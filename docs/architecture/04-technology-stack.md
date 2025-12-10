# The Cut Club - Technology Stack Evaluation

## iOS Application Stack

### Core Technologies

#### Programming Language
**Decision: Swift 5.9+**

| Criteria | Rating | Notes |
|----------|--------|-------|
| Performance | 5/5 | Native performance, no bridge overhead |
| Developer Experience | 5/5 | Modern syntax, type safety, playgrounds |
| Ecosystem | 5/5 | First-class Apple support, SPM integration |
| Future-proof | 5/5 | Apple's primary language |
| Hiring Pool | 4/5 | Growing talent pool |

**Alternative Considered**: Objective-C (Rejected - legacy, declining usage)

#### UI Framework
**Decision: SwiftUI (Primary) + UIKit (Selective)**

| Feature | SwiftUI | UIKit | Choice |
|---------|---------|-------|--------|
| Declarative UI | Yes | No | SwiftUI |
| Animations | Native, fluid | Manual | SwiftUI |
| Complex Lists | Good (iOS 17+) | Excellent | Hybrid |
| Camera/Video | Limited | Excellent | UIKit |
| Map Integration | Good | Excellent | Hybrid |
| Learning Curve | Moderate | Steep | SwiftUI |

**Strategy**: SwiftUI for 90% of UI, UIKit for:
- Advanced camera controls (barber post creation)
- Complex video playback
- Custom collection view layouts
- Legacy library integration

#### Architecture Pattern
**Decision: Clean Architecture + MVVM**

```
┌─────────────────────────────────────────┐
│         Presentation Layer              │
│    (SwiftUI Views + ViewModels)         │
└────────────────┬────────────────────────┘
                 │
┌────────────────┴────────────────────────┐
│         Domain Layer                    │
│    (Use Cases + Entities + Protocols)   │
└────────────────┬────────────────────────┘
                 │
┌────────────────┴────────────────────────┐
│         Data Layer                      │
│  (Repositories + Network + Local DB)    │
└─────────────────────────────────────────┘
```

**Benefits**:
- Testability (80%+ coverage achievable)
- Separation of concerns
- Independent of frameworks
- Scalable for large teams

#### Dependency Injection
**Decision: Swift Package Manager + Protocol-based DI**

```swift
protocol BookingServiceProtocol {
    func createBooking(_ request: BookingRequest) async throws -> Booking
}

class BookingViewModel: ObservableObject {
    private let bookingService: BookingServiceProtocol

    init(bookingService: BookingServiceProtocol = BookingService()) {
        self.bookingService = bookingService
    }
}
```

**Alternative Considered**:
- Swinject (Rejected - adds complexity for limited benefit)
- Needle (Rejected - compile-time DI overkill for this scale)

### Key Dependencies (Swift Package Manager)

#### Networking
**Alamofire** (5.8+)
- Reasons: Type-safe requests, built-in retry logic, certificate pinning
- Alternative: URLSession native (considered, but Alamofire adds valuable abstractions)

#### Image Loading
**Kingfisher** (7.10+)
- Reasons: Async/await support, caching, animated image support
- Alternative: SDWebImage (equally good, Kingfisher has better Swift ergonomics)

#### Local Storage
**Core Data + GRDB** (hybrid approach)
- Core Data: User profiles, bookings, cached data
- GRDB: High-performance queries, analytics data
- Alternative: Realm (Rejected - Firebase integration is better with Core Data)

#### Firebase SDK
**Firebase iOS SDK** (10.20+)
- FirebaseAuth
- FirebaseFirestore
- FirebaseStorage
- FirebaseMessaging (FCM)
- FirebaseAnalytics
- FirebaseCrashlytics

#### Real-time Communication
**Starscream** (4.0+)
- WebSocket client for live features
- Alternative: Native URLSessionWebSocketTask (considered, Starscream more feature-rich)

#### Animations
**Lottie-iOS** (4.3+)
- Complex animations (slot machine, rewards)
- Alternative: SwiftUI native animations (used for 80% of animations)

#### Video Playback
**AVKit + AVFoundation** (Native)
- Social feed video posts
- No third-party needed

#### Push Notifications
**Firebase Cloud Messaging** (FCM)
- Cross-platform consistency
- Advanced targeting

#### Analytics
**Mixpanel** (4.2+) + **Firebase Analytics**
- Mixpanel: User behavior, funnels, retention
- Firebase: Performance, crashes, app usage
- Alternative: Amplitude (equally good, went with Mixpanel for pricing)

#### Error Tracking
**Sentry** (8.15+)
- Real-time error tracking
- Performance monitoring
- Release tracking
- Alternative: Firebase Crashlytics (using both - Sentry for detailed debugging)

#### Payments
**Stripe iOS SDK** (23.26+)
- Apple Pay integration
- Card tokenization
- 3D Secure 2

#### Maps
**MapKit** (Native)
- Shop locations
- Distance calculations
- No third-party needed

#### Haptics
**Core Haptics** (Native)
- Custom haptic patterns
- Reward wheel feedback
- Booking confirmations

## Backend Stack

### Option 1: Firebase (Recommended for MVP)

#### Pros
- 3-6 month faster development
- Automatic scaling
- Built-in security rules
- Real-time out of the box
- Authentication included
- Lower initial cost

#### Cons
- Vendor lock-in
- Cost at scale (20K+ users)
- Limited complex queries
- Cold start latency (Cloud Functions)

#### Firebase Services

| Service | Purpose | Monthly Cost (10K users) |
|---------|---------|--------------------------|
| Firebase Auth | User authentication | Free |
| Cloud Firestore | Primary database | $150-300 |
| Cloud Storage | Images/videos | $50-100 |
| Cloud Functions | Backend logic | $100-200 |
| Firebase Hosting | Admin panel | $5 |
| Cloud Messaging | Push notifications | Free |
| Analytics | App analytics | Free |
| Crashlytics | Error tracking | Free |
| **Total** | | **$305-605/mo** |

#### Firebase Architecture

```typescript
// Cloud Functions (TypeScript)
export const createBooking = functions.https.onCall(async (data, context) => {
  // Validate user is authenticated
  if (!context.auth) {
    throw new functions.https.HttpsError('unauthenticated', 'Must be logged in');
  }

  // Check availability
  const isAvailable = await checkBarberAvailability(data.barberId, data.scheduledTime);

  if (!isAvailable) {
    throw new functions.https.HttpsError('failed-precondition', 'Time slot unavailable');
  }

  // Create booking
  const bookingRef = await admin.firestore().collection('bookings').add({
    userId: context.auth.uid,
    barberId: data.barberId,
    scheduledDate: data.scheduledDate,
    status: 'pending',
    createdAt: admin.firestore.FieldValue.serverTimestamp()
  });

  // Send notifications
  await sendBookingConfirmation(context.auth.uid, bookingRef.id);

  return { bookingId: bookingRef.id };
});
```

### Option 2: Custom Backend (Scale Path)

#### When to Consider
- 50K+ monthly active users
- Firebase costs exceed $2K/month
- Need complex queries/joins
- Require custom ML models
- Multi-tenant requirements

#### Technology Choices

**Runtime**: Node.js 20 LTS + TypeScript 5.3
- Pros: JavaScript everywhere, huge ecosystem, async by nature
- Alternatives: Python (FastAPI), Go, Rust (all viable, Node.js for team velocity)

**API Framework**: Express.js + GraphQL (Apollo Server)
- Express for REST, GraphQL for complex data fetching
- Alternative: tRPC (type-safe, but less mature)

**Primary Database**: PostgreSQL 16
- Bookings, users, transactions (ACID requirements)
- Schema:
  - Strong relationships (users -> bookings -> services)
  - Complex queries with JOINs
  - JSONB for flexible fields

**Secondary Database**: MongoDB 7
- Social feed, posts, comments
- Schema-less for rapid iteration
- Geospatial queries (shop locations)

**Cache Layer**: Redis 7
- Session storage
- Leaderboards (sorted sets)
- Queue status
- Rate limiting

**Message Queue**: Bull (Redis-based)
- Background jobs
- Email/SMS sending
- Analytics processing
- Image resizing

**Real-time**: Socket.io
- Queue updates
- Booking status
- DJ mode voting
- Live feed

**Search**: Elasticsearch 8
- Barber/shop search
- Post search with tags
- Autocomplete

**Storage**: AWS S3 / Google Cloud Storage
- Images, videos
- CDN integration (CloudFront/Cloud CDN)

**Hosting**: Kubernetes (GKE / EKS)
- Horizontal scaling
- Zero-downtime deploys
- Multi-region support

#### Custom Backend Cost (10K users)

| Service | Provider | Monthly Cost |
|---------|----------|--------------|
| Compute (3 nodes) | GCP/AWS | $300 |
| PostgreSQL | Cloud SQL/RDS | $150 |
| MongoDB | Atlas | $100 |
| Redis | Cloud Memorystore | $50 |
| Storage + CDN | GCS/S3 + CDN | $100 |
| Load Balancer | Cloud LB | $20 |
| **Total** | | **$720/mo** |

**Break-even**: ~25K users (Firebase becomes more expensive)

### Technology Decision Matrix

| Criteria | Firebase | Custom Backend | Winner (MVP) |
|----------|----------|----------------|--------------|
| Time to Market | 3-4 months | 6-9 months | Firebase |
| Initial Cost | $300-600/mo | $720/mo | Firebase |
| Cost at 100K users | $5K+/mo | $2K/mo | Custom |
| Scalability | Good | Excellent | Custom |
| Flexibility | Limited | Full control | Custom |
| Maintenance | Low | Medium-High | Firebase |
| Team Size Needed | 2-3 | 4-6 | Firebase |
| Real-time Features | Excellent | Good | Firebase |
| Complex Queries | Poor | Excellent | Custom |
| Vendor Lock-in | High | None | Custom |

**Recommendation**: Start with Firebase, migrate to custom backend at 30K-50K users or $2K/mo Firebase costs.

## Third-Party Services

### Payments
**Stripe Connect** (Marketplace Model)

| Feature | Stripe | Square | PayPal | Winner |
|---------|--------|--------|--------|--------|
| Apple Pay | Yes | Yes | Yes | Tie |
| Platform Fees | 2.9% + 30¢ | 2.6% + 10¢ | 3.5% + 15¢ | Square |
| Connect Support | Excellent | Good | Poor | Stripe |
| iOS SDK Quality | Excellent | Good | Fair | Stripe |
| Split Payments | Native | Workaround | Complex | Stripe |
| Dashboard | Excellent | Good | Fair | Stripe |

**Decision**: Stripe Connect
- Best developer experience
- Native split payments (shop vs barber)
- Superior documentation
- Better fraud prevention

### SMS/Voice
**Twilio**

| Feature | Twilio | AWS SNS | MessageBird | Winner |
|---------|--------|---------|-------------|--------|
| Reliability | 99.95% | 99.9% | 99.9% | Twilio |
| Pricing | $0.0079/SMS | $0.0065/SMS | $0.0075/SMS | AWS SNS |
| Features | Excellent | Basic | Good | Twilio |
| International | Excellent | Good | Excellent | Tie |

**Decision**: Twilio
- Better deliverability
- Rich features (two-way SMS, scheduling)
- Worth slight price premium

### Email
**SendGrid**

| Feature | SendGrid | AWS SES | Mailgun | Winner |
|---------|----------|---------|---------|--------|
| Deliverability | Excellent | Excellent | Good | Tie |
| Templates | Excellent | Basic | Good | SendGrid |
| Analytics | Excellent | Basic | Good | SendGrid |
| Pricing | $15/50K | $0.10/1K | $35/50K | AWS SES |

**Decision**: SendGrid
- Better templates and analytics
- Easier setup than SES
- Reasonable pricing

### Analytics
**Mixpanel** (primary) + **Firebase Analytics** (secondary)

**Mixpanel**:
- User funnels
- Retention analysis
- A/B testing
- Cohort analysis
- $89/mo (10K users)

**Firebase Analytics**:
- Free
- App performance
- Crash analytics
- Basic user metrics

### Error Tracking
**Sentry**

| Feature | Sentry | Bugsnag | Rollbar | Winner |
|---------|--------|---------|---------|--------|
| Swift Support | Excellent | Good | Fair | Sentry |
| Performance Monitoring | Yes | No | No | Sentry |
| Release Tracking | Excellent | Good | Good | Sentry |
| Pricing | $26/mo | $59/mo | $25/mo | Sentry |

**Decision**: Sentry
- Best Swift/iOS support
- Performance monitoring included
- Source map support

### CDN
**Cloudflare** (if custom backend) or **Firebase CDN** (if Firebase)

**Cloudflare**:
- $20/mo (Pro plan)
- Global edge network
- DDoS protection
- Image optimization

**Firebase CDN**:
- Included with Firebase Hosting
- Google's CDN
- Automatic HTTPS

## Development Tools

### CI/CD
**GitHub Actions** + **Fastlane**

```yaml
# .github/workflows/ios.yml
name: iOS CI
on: [push, pull_request]
jobs:
  test:
    runs-on: macos-14
    steps:
      - uses: actions/checkout@v4
      - name: Run tests
        run: fastlane test
      - name: Build
        run: fastlane build
      - name: Deploy to TestFlight
        if: github.ref == 'refs/heads/main'
        run: fastlane beta
```

### Testing
- **XCTest**: Unit tests
- **Quick/Nimble**: BDD-style tests
- **SnapshotTesting**: UI regression tests
- **SwiftLint**: Code quality
- **SonarQube**: Code analysis

### Monitoring
- **New Relic**: APM (if custom backend)
- **Firebase Performance**: iOS app performance
- **Sentry**: Error tracking
- **Mixpanel**: User analytics

### Feature Flags
**LaunchDarkly** or **Firebase Remote Config**
- A/B testing
- Gradual rollouts
- Kill switches

### Documentation
- **SwiftDoc**: Code documentation
- **Postman**: API documentation
- **Notion**: Product docs

## Infrastructure (Custom Backend Path)

### Kubernetes Architecture

```yaml
# Simplified k8s structure
apiVersion: apps/v1
kind: Deployment
metadata:
  name: cutclub-api
spec:
  replicas: 3
  template:
    spec:
      containers:
      - name: api
        image: cutclub/api:latest
        resources:
          requests:
            memory: "512Mi"
            cpu: "500m"
          limits:
            memory: "1Gi"
            cpu: "1000m"
---
apiVersion: v1
kind: Service
metadata:
  name: cutclub-api
spec:
  type: LoadBalancer
  ports:
  - port: 443
    targetPort: 8080
```

### Scaling Strategy
- **Horizontal Pod Autoscaling**: 3-20 pods based on CPU/memory
- **Database Read Replicas**: Scale reads independently
- **CDN**: Edge caching for static content
- **Redis Cluster**: Distributed cache

### Disaster Recovery
- **Backup**: Daily automated backups (Firestore/PostgreSQL)
- **RTO**: 15 minutes (multi-region failover)
- **RPO**: 5 minutes (point-in-time recovery)

## Cost Comparison Summary (Monthly)

| Users | Firebase | Custom Backend | Savings |
|-------|----------|----------------|---------|
| 1K | $50 | $720 | -$670 |
| 10K | $500 | $720 | -$220 |
| 25K | $1,500 | $1,000 | $500 |
| 50K | $3,500 | $1,500 | $2,000 |
| 100K | $8,000 | $2,500 | $5,500 |

## Migration Strategy (Firebase → Custom)

### Phase 1: Preparation (Months 1-3)
1. Abstract data access layer in iOS app
2. Create repository pattern
3. Use protocols for all external services

### Phase 2: Parallel Run (Months 4-6)
1. Build custom backend
2. Dual-write to both systems
3. Verify data consistency

### Phase 3: Migration (Month 7)
1. Gradual traffic shift (10% → 50% → 100%)
2. Monitor error rates
3. Keep Firebase as backup

### Phase 4: Cleanup (Month 8)
1. Remove Firebase dependencies
2. Archive data
3. Cancel Firebase subscription

## Next: Security & Compliance
See `/docs/architecture/05-security.md` for security architecture.
