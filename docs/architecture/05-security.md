# The Cut Club - Security Architecture

## Security Principles

### Core Tenets
1. **Defense in Depth**: Multiple layers of security
2. **Least Privilege**: Minimal access rights
3. **Zero Trust**: Verify everything, trust nothing
4. **Privacy by Design**: Data protection built-in
5. **Security Transparency**: Clear communication with users

## Authentication & Authorization

### User Authentication Flow

```
┌─────────────┐
│   iOS App   │
└──────┬──────┘
       │ 1. User taps "Sign In"
       ▼
┌──────────────────────────┐
│ Sign in with Apple       │
│ (or Email + Biometric)   │
└──────┬───────────────────┘
       │ 2. Apple ID Token
       ▼
┌──────────────────────────┐
│ Firebase Auth            │
│ • Verify Apple token     │
│ • Create/fetch user      │
│ • Generate JWT           │
└──────┬───────────────────┘
       │ 3. JWT Token
       ▼
┌──────────────────────────┐
│ iOS App (Keychain)       │
│ • Store access token     │
│ • Store refresh token    │
│ • Enable biometric lock  │
└──────────────────────────┘
```

### Authentication Methods

#### 1. Sign in with Apple (Primary)
**Implementation**:
```swift
import AuthenticationServices

class AuthService {
    func signInWithApple() async throws -> User {
        let provider = ASAuthorizationAppleIDProvider()
        let request = provider.createRequest()
        request.requestedScopes = [.fullName, .email]

        // Present authorization controller
        let result = try await performSignIn(request)

        // Exchange Apple token for Firebase token
        let credential = OAuthProvider.credential(
            withProviderID: "apple.com",
            idToken: result.identityToken,
            rawNonce: nonce
        )

        let authResult = try await Auth.auth().signIn(with: credential)
        return authResult.user
    }
}
```

**Security Features**:
- Private email relay
- Two-factor authentication
- Revocation detection
- Anti-replay nonce

**Storage**:
```swift
// Store tokens in Keychain
try Keychain.standard.set(
    accessToken,
    key: "auth.accessToken",
    accessibility: .whenUnlockedThisDeviceOnly
)

// Store user ID for biometric access
try Keychain.standard.set(
    userId,
    key: "auth.userId",
    accessibility: .whenUnlocked,
    authenticationPolicy: .biometryCurrentSet
)
```

#### 2. Email + Password (Secondary)
**Requirements**:
- Minimum 12 characters
- Uppercase + lowercase + number + special char
- Password strength meter
- Breach detection (HaveIBeenPwned API)

**Biometric Protection**:
```swift
import LocalAuthentication

class BiometricAuth {
    func authenticateUser() async throws -> Bool {
        let context = LAContext()
        context.localizedCancelTitle = "Use Passcode"

        guard context.canEvaluatePolicy(
            .deviceOwnerAuthenticationWithBiometrics,
            error: nil
        ) else {
            throw BiometricError.notAvailable
        }

        return try await context.evaluatePolicy(
            .deviceOwnerAuthenticationWithBiometrics,
            localizedReason: "Authenticate to access The Cut Club"
        )
    }
}
```

#### 3. Phone Number (SMS Verification)
**Flow**:
1. User enters phone number
2. Firebase sends 6-digit code via Twilio
3. User enters code within 5 minutes
4. Account verified

**Security**:
- Rate limiting (3 attempts per 10 minutes)
- Short code expiration (5 minutes)
- Device fingerprinting (prevent abuse)

### JWT Token Structure

```json
{
  "header": {
    "alg": "RS256",
    "typ": "JWT",
    "kid": "key-id-123"
  },
  "payload": {
    "sub": "user_123",
    "iss": "https://api.thecutclub.com",
    "aud": "thecutclub-ios",
    "exp": 1638360000,
    "iat": 1638356400,
    "email": "user@example.com",
    "tier": "black-card",
    "permissions": ["booking:create", "booking:read", "booking:update"],
    "device_id": "device_fingerprint_hash"
  }
}
```

**Token Lifetimes**:
- Access Token: 1 hour
- Refresh Token: 30 days
- Black Card Session: 24 hours (no refresh needed)

### Authorization Model

#### Role-Based Access Control (RBAC)

```typescript
enum Role {
  CUSTOMER = 'customer',
  BARBER = 'barber',
  SHOP_MANAGER = 'shop_manager',
  ADMIN = 'admin'
}

enum Permission {
  // Booking
  BOOKING_CREATE = 'booking:create',
  BOOKING_READ = 'booking:read',
  BOOKING_UPDATE = 'booking:update',
  BOOKING_DELETE = 'booking:delete',
  BOOKING_MANAGE_ALL = 'booking:manage_all',

  // Posts
  POST_CREATE = 'post:create',
  POST_UPDATE = 'post:update',
  POST_DELETE = 'post:delete',
  POST_MANAGE_ALL = 'post:manage_all',

  // VIP
  VIP_ACCESS = 'vip:access',
  VIP_LOUNGE = 'vip:lounge',

  // Admin
  ADMIN_USERS = 'admin:users',
  ADMIN_ANALYTICS = 'admin:analytics'
}

const rolePermissions: Record<Role, Permission[]> = {
  [Role.CUSTOMER]: [
    Permission.BOOKING_CREATE,
    Permission.BOOKING_READ,
    Permission.BOOKING_UPDATE
  ],
  [Role.BARBER]: [
    Permission.BOOKING_READ,
    Permission.BOOKING_MANAGE_ALL,
    Permission.POST_CREATE,
    Permission.POST_UPDATE
  ],
  [Role.SHOP_MANAGER]: [
    Permission.BOOKING_MANAGE_ALL,
    Permission.POST_MANAGE_ALL,
    Permission.ADMIN_ANALYTICS
  ],
  [Role.ADMIN]: Object.values(Permission)
};
```

#### Firebase Security Rules

```javascript
// Firestore Security Rules
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // Helper functions
    function isAuthenticated() {
      return request.auth != null;
    }

    function isOwner(userId) {
      return isAuthenticated() && request.auth.uid == userId;
    }

    function hasRole(role) {
      return isAuthenticated() &&
             get(/databases/$(database)/documents/users/$(request.auth.uid))
               .data.role == role;
    }

    function isBlackCard() {
      return isAuthenticated() &&
             get(/databases/$(database)/documents/users/$(request.auth.uid))
               .data.tier == 'black-card';
    }

    // User documents
    match /users/{userId} {
      allow read: if isAuthenticated();
      allow write: if isOwner(userId);
      allow update: if isOwner(userId) &&
                       !request.resource.data.diff(resource.data)
                         .affectedKeys().hasAny(['points', 'tier']);

      // Achievement progress (subcollection)
      match /achievement_progress/{achievementId} {
        allow read: if isOwner(userId);
        allow write: if false; // Server-side only
      }
    }

    // Bookings
    match /bookings/{bookingId} {
      allow read: if isAuthenticated() &&
                     (isOwner(resource.data.userId) ||
                      hasRole('barber') ||
                      hasRole('shop_manager'));

      allow create: if isAuthenticated() &&
                       request.resource.data.userId == request.auth.uid;

      allow update: if isOwner(resource.data.userId) ||
                       hasRole('barber');

      allow delete: if hasRole('shop_manager') ||
                       hasRole('admin');
    }

    // Posts
    match /posts/{postId} {
      allow read: if true; // Public feed

      allow create: if hasRole('barber') &&
                       request.resource.data.barberId == request.auth.uid;

      allow update, delete: if hasRole('barber') &&
                               resource.data.barberId == request.auth.uid;

      // Comments (subcollection)
      match /comments/{commentId} {
        allow read: if true;
        allow create: if isAuthenticated();
        allow update, delete: if isOwner(resource.data.userId);
      }
    }

    // VIP lounge access
    match /vip_access/{accessId} {
      allow read: if isOwner(resource.data.userId) && isBlackCard();
      allow write: if false; // Server-side only
    }

    // Rewards
    match /rewards/{rewardId} {
      allow read: if isOwner(resource.data.userId);
      allow write: if false; // Server-side only
    }
  }
}
```

## Data Security

### Encryption

#### Data at Rest
**Firebase Firestore**:
- AES-256 encryption (Google-managed keys)
- Optional CMEK (Customer-Managed Encryption Keys) for enterprise

**iOS Local Storage**:
```swift
// Keychain for sensitive data
class SecureStorage {
    static func store(
        _ value: String,
        forKey key: String,
        protection: KeychainAccessibility = .whenUnlockedThisDeviceOnly
    ) throws {
        let query: [String: Any] = [
            kSecClass as String: kSecClassGenericPassword,
            kSecAttrAccount as String: key,
            kSecValueData as String: value.data(using: .utf8)!,
            kSecAttrAccessible as String: protection.rawValue
        ]

        let status = SecItemAdd(query as CFDictionary, nil)
        guard status == errSecSuccess else {
            throw KeychainError.storeFailed
        }
    }
}

// Core Data encryption
let container = NSPersistentContainer(name: "TheCutClub")
container.loadPersistentStores { description, error in
    description.setOption(
        FileProtectionType.complete as NSObject,
        forKey: NSPersistentStoreFileProtectionKey
    )
}
```

#### Data in Transit
**TLS 1.3**:
- All API communication over HTTPS
- Certificate pinning for critical endpoints

```swift
class APIClient {
    private let session: URLSession

    init() {
        let configuration = URLSessionConfiguration.default
        configuration.tlsMinimumSupportedProtocolVersion = .TLSv13

        self.session = URLSession(
            configuration: configuration,
            delegate: CertificatePinningDelegate(),
            delegateQueue: nil
        )
    }
}

// Certificate Pinning
class CertificatePinningDelegate: NSObject, URLSessionDelegate {
    func urlSession(
        _ session: URLSession,
        didReceive challenge: URLAuthenticationChallenge,
        completionHandler: @escaping (URLSession.AuthChallengeDisposition, URLCredential?) -> Void
    ) {
        guard let serverTrust = challenge.protectionSpace.serverTrust else {
            completionHandler(.cancelAuthenticationChallenge, nil)
            return
        }

        let pinnedCertificates = [/* SHA256 hashes */]

        // Verify certificate
        if verifyCertificate(serverTrust, against: pinnedCertificates) {
            completionHandler(.useCredential, URLCredential(trust: serverTrust))
        } else {
            completionHandler(.cancelAuthenticationChallenge, nil)
        }
    }
}
```

### Sensitive Data Handling

#### PII (Personally Identifiable Information)
**Fields**:
- Full name
- Email
- Phone number
- Date of birth
- Payment information

**Protection**:
1. **Minimization**: Collect only what's needed
2. **Encryption**: At rest and in transit
3. **Access Control**: Strict RBAC
4. **Retention**: Auto-delete after 2 years of inactivity
5. **Anonymization**: Analytics use hashed IDs

#### Payment Information
**PCI DSS Compliance via Stripe**:
- No card data stored on our servers
- Stripe tokenization
- PCI SAQ-A (simplest compliance level)

```swift
// Stripe iOS SDK
import Stripe

class PaymentService {
    func createPaymentMethod(cardDetails: STPCardParams) async throws -> String {
        // Card data never touches our servers
        let paymentMethod = try await STPAPIClient.shared.createPaymentMethod(
            with: cardDetails
        )

        // Only store Stripe token
        return paymentMethod.stripeId
    }
}
```

### Data Loss Prevention (DLP)

#### Screenshot Protection
```swift
// Protect sensitive screens from screenshots
class SecureViewController: UIViewController {
    private let securityView = UIView()

    override func viewDidLoad() {
        super.viewDidLoad()

        // Blur screen when app enters background
        NotificationCenter.default.addObserver(
            self,
            selector: #selector(hideSecureContent),
            name: UIApplication.willResignActiveNotification,
            object: nil
        )
    }

    @objc private func hideSecureContent() {
        // Add blur effect
        let blurEffect = UIBlurEffect(style: .regular)
        let blurView = UIVisualEffectView(effect: blurEffect)
        blurView.frame = view.bounds
        view.addSubview(blurView)
    }
}
```

#### Screen Recording Detection
```swift
class SecurityMonitor {
    func detectScreenRecording() -> Bool {
        return UIScreen.main.isCaptured
    }

    func observeScreenCapture() {
        NotificationCenter.default.addObserver(
            forName: UIScreen.capturedDidChangeNotification,
            object: nil,
            queue: .main
        ) { [weak self] _ in
            if UIScreen.main.isCaptured {
                self?.handleScreenRecording()
            }
        }
    }

    private func handleScreenRecording() {
        // Show warning or blur sensitive content
        Logger.security.warning("Screen recording detected")
        Analytics.track("security_screen_recording_detected")
    }
}
```

## Network Security

### API Security

#### Rate Limiting
```typescript
// Express middleware
import rateLimit from 'express-rate-limit';

const limiter = rateLimit({
  windowMs: 1 * 60 * 1000, // 1 minute
  max: (req) => {
    // Black Card users get higher limits
    if (req.user?.tier === 'black-card') return 1000;
    if (req.user?.role === 'barber') return 500;
    return 100;
  },
  message: 'Too many requests, please try again later',
  standardHeaders: true,
  legacyHeaders: false,
});

app.use('/api/', limiter);
```

#### Request Validation
```typescript
import { body, validationResult } from 'express-validator';

app.post('/bookings',
  authenticate,
  [
    body('barberId').isUUID(),
    body('scheduledDate').isISO8601(),
    body('scheduledTime').matches(/^([0-1]?[0-9]|2[0-3]):[0-5][0-9]$/),
    body('serviceIds').isArray({ min: 1, max: 5 }),
  ],
  async (req, res) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(400).json({ errors: errors.array() });
    }

    // Process booking...
  }
);
```

#### SQL Injection Prevention
```typescript
// Use parameterized queries
const booking = await db.query(
  'SELECT * FROM bookings WHERE id = $1 AND user_id = $2',
  [bookingId, userId]
);

// OR use ORM (Prisma, TypeORM)
const booking = await prisma.booking.findFirst({
  where: {
    id: bookingId,
    userId: userId
  }
});
```

#### NoSQL Injection Prevention
```typescript
// Firestore safe queries
const bookingsRef = firestore.collection('bookings');

// SAFE: Parameterized
const bookings = await bookingsRef
  .where('userId', '==', userId)
  .where('status', '==', 'confirmed')
  .get();

// UNSAFE: String interpolation
// const bookings = await bookingsRef
//   .where('userId', '==', req.query.userId) // VULNERABLE
//   .get();
```

### DDoS Protection

#### Cloudflare (if custom backend)
- Rate limiting
- Bot detection
- Challenge pages
- Geographic blocking

#### Firebase (built-in)
- Automatic DDoS mitigation
- Google Cloud Armor integration
- No configuration needed

## Application Security

### Code Security

#### Dependency Management
```bash
# Swift Package Manager
swift package show-dependencies
swift package audit

# Monitor vulnerabilities
npm install -g swift-package-audit
swift-package-audit
```

#### Static Analysis
```bash
# SwiftLint
swiftlint --strict

# SonarQube
sonar-scanner \
  -Dsonar.projectKey=thecutclub-ios \
  -Dsonar.sources=. \
  -Dsonar.host.url=https://sonar.thecutclub.com
```

### Reverse Engineering Protection

#### Code Obfuscation
```swift
// Use Swift's built-in optimization
// Build Settings:
// - Optimization Level: Optimize for Speed [-O]
// - Compilation Mode: Whole Module
// - Strip Debug Symbols: Yes (Release)

// Obfuscate sensitive strings
class SecurityConstants {
    private static let key = [0x41, 0x50, 0x49, 0x5F, 0x4B, 0x45, 0x59]

    static var apiKey: String {
        return String(bytes: key, encoding: .utf8) ?? ""
    }
}
```

#### Jailbreak Detection
```swift
class JailbreakDetector {
    static func isJailbroken() -> Bool {
        // Check for suspicious files
        let suspiciousPaths = [
            "/Applications/Cydia.app",
            "/Library/MobileSubstrate/MobileSubstrate.dylib",
            "/bin/bash",
            "/usr/sbin/sshd",
            "/etc/apt"
        ]

        for path in suspiciousPaths {
            if FileManager.default.fileExists(atPath: path) {
                return true
            }
        }

        // Check if can write outside sandbox
        let testPath = "/private/test_jailbreak.txt"
        do {
            try "test".write(toFile: testPath, atomically: true, encoding: .utf8)
            try FileManager.default.removeItem(atPath: testPath)
            return true
        } catch {
            // Cannot write, good
        }

        // Check suspicious environment variables
        if getenv("DYLD_INSERT_LIBRARIES") != nil {
            return true
        }

        return false
    }

    static func handleJailbreak() {
        Logger.security.critical("Jailbreak detected")

        // Show warning
        // Limit functionality
        // Or refuse to run (extreme)
    }
}
```

## Privacy & Compliance

### GDPR Compliance

#### Right to Access
**Endpoint**: `GET /users/me/data-export`
```json
{
  "requestedAt": "2025-12-09T12:00:00Z",
  "exportUrl": "https://exports.thecutclub.com/user_123.zip",
  "expiresAt": "2025-12-16T12:00:00Z",
  "includes": [
    "profile",
    "bookings",
    "posts",
    "achievements",
    "rewards",
    "transactions"
  ]
}
```

#### Right to Deletion
**Endpoint**: `DELETE /users/me/account`
```typescript
async function deleteUserAccount(userId: string) {
  // 1. Anonymize user data
  await db.users.update(userId, {
    email: `deleted_${userId}@deleted.com`,
    firstName: 'Deleted',
    lastName: 'User',
    phoneNumber: null,
    avatarUrl: null,
    isActive: false,
    deletedAt: new Date()
  });

  // 2. Remove PII from related records
  await db.bookings.updateMany(
    { userId },
    { notes: null }
  );

  // 3. Keep transaction history (legal requirement)
  // Do NOT delete bookings/transactions

  // 4. Remove from social features
  await db.posts.updateMany(
    { userId },
    { isDeleted: true }
  );

  // 5. Schedule full deletion after 30 days
  await scheduleJob('delete-user-data', { userId }, { delay: 30 * 86400000 });
}
```

#### Consent Management
```swift
class ConsentManager {
    func requestConsent() async -> Bool {
        let consent = await showConsentDialog()

        if consent.analytics {
            Analytics.enable()
        }

        if consent.marketing {
            NotificationManager.enablePromotions()
        }

        // Store consent
        UserDefaults.standard.set(consent.analytics, forKey: "consent.analytics")
        UserDefaults.standard.set(consent.marketing, forKey: "consent.marketing")

        return consent.required
    }
}
```

### CCPA Compliance

#### Do Not Sell
```swift
// App Settings
class PrivacySettings {
    @AppStorage("privacy.doNotSell") var doNotSell: Bool = false

    func updateDoNotSell(_ value: Bool) {
        doNotSell = value

        // Notify backend
        Task {
            try await API.updatePrivacyPreferences(doNotSell: value)
        }

        // Update analytics
        if value {
            Analytics.disable()
            Mixpanel.optOut()
        }
    }
}
```

## Incident Response

### Security Monitoring

#### Logging
```swift
import OSLog

extension Logger {
    static let security = Logger(
        subsystem: "com.thecutclub.ios",
        category: "security"
    )
}

// Usage
Logger.security.warning("Failed authentication attempt", metadata: [
    "userId": userId,
    "reason": "invalid_credentials",
    "ipAddress": ipAddress
])
```

#### Anomaly Detection
```typescript
// Backend monitoring
class SecurityMonitor {
  async detectAnomalies(userId: string) {
    const recent = await getRecentActivity(userId);

    // Check for suspicious patterns
    if (recent.failedLogins > 5) {
      await lockAccount(userId, '15m');
      await notify.security('Multiple failed logins', { userId });
    }

    if (recent.bookingsCreated > 10) {
      await notify.security('Excessive bookings', { userId });
    }

    // Geo-anomaly
    const locations = recent.requests.map(r => r.ipLocation);
    if (hasSuspiciousTravel(locations)) {
      await requireMFA(userId);
    }
  }
}
```

### Incident Response Plan

#### Severity Levels
1. **Critical**: Data breach, authentication bypass
2. **High**: DDoS, privilege escalation
3. **Medium**: Rate limit bypass, spam
4. **Low**: Failed authentication, suspicious activity

#### Response Timeline
- **Critical**: 15 minutes
- **High**: 1 hour
- **Medium**: 4 hours
- **Low**: 24 hours

#### Response Steps
1. **Detect**: Automated monitoring alerts
2. **Contain**: Disable affected systems
3. **Investigate**: Analyze logs and scope
4. **Remediate**: Fix vulnerability
5. **Recover**: Restore services
6. **Review**: Post-mortem analysis

## Security Checklist

### Pre-Launch
- [ ] Penetration testing completed
- [ ] Security audit by third party
- [ ] All dependencies up to date
- [ ] Certificate pinning implemented
- [ ] Rate limiting configured
- [ ] Firestore security rules tested
- [ ] Encryption at rest enabled
- [ ] Jailbreak detection active
- [ ] Privacy policy published
- [ ] Terms of service updated
- [ ] GDPR/CCPA compliance verified
- [ ] Incident response plan documented
- [ ] Backup and recovery tested

### Ongoing
- [ ] Monthly dependency audits
- [ ] Quarterly penetration tests
- [ ] Continuous security monitoring
- [ ] Regular security training
- [ ] Vulnerability disclosure program

## Next: Scalability & Performance
See `/docs/architecture/06-scalability.md` for scaling strategy.
