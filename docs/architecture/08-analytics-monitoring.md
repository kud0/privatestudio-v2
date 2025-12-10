# The Cut Club - Analytics & Monitoring Strategy

## Analytics Overview

### Core Objectives
1. **Product Analytics**: Understand user behavior and feature adoption
2. **Business Metrics**: Track revenue, retention, and growth
3. **Technical Monitoring**: Ensure system health and performance
4. **Gamification Metrics**: Measure engagement with game mechanics
5. **Social Metrics**: Track content creation and interactions

### Analytics Stack

| Tool | Purpose | Cost (10K users) |
|------|---------|------------------|
| Firebase Analytics | Free app analytics, basic funnels | Free |
| Mixpanel | Advanced user analytics, cohorts | $89/mo |
| Amplitude | Alternative to Mixpanel | $61/mo |
| Sentry | Error tracking, performance | $26/mo |
| Firebase Crashlytics | Crash reporting | Free |
| Firebase Performance | App performance monitoring | Free |
| LogRocket | Session replay (optional) | $99/mo |
| Datadog | Infrastructure monitoring | $150/mo |

**Recommended MVP Stack**:
- Firebase Analytics (free, basic)
- Mixpanel (advanced, $89/mo)
- Sentry (errors, $26/mo)
- **Total**: $115/mo

## Event Tracking

### Event Taxonomy

#### User Lifecycle Events
```typescript
// Registration & Onboarding
{
  "signup_started": {},
  "signup_completed": {
    method: "apple" | "email" | "phone",
    referralSource?: string
  },
  "onboarding_step_completed": {
    step: number,
    stepName: string
  },
  "onboarding_completed": {
    durationSeconds: number
  },

  // Profile
  "profile_updated": {
    fields: string[] // ["avatarUrl", "displayName"]
  },
  "preferred_barber_set": {
    barberId: string
  }
}
```

#### Booking Events
```typescript
{
  // Discovery
  "shop_searched": {
    query: string,
    filters: object,
    resultsCount: number
  },
  "shop_viewed": {
    shopId: string,
    source: "search" | "map" | "deeplink"
  },
  "barber_viewed": {
    barberId: string,
    shopId: string
  },

  // Booking Flow
  "booking_started": {
    barberId: string,
    shopId: string,
    serviceIds: string[]
  },
  "booking_time_selected": {
    date: string,
    time: string,
    isBlackCardSlot: boolean
  },
  "booking_payment_initiated": {
    amount: number,
    serviceIds: string[]
  },
  "booking_created": {
    bookingId: string,
    barberId: string,
    shopId: string,
    totalAmount: number,
    isBlackCard: boolean,
    timeToComplete: number // ms from start
  },
  "booking_cancelled": {
    bookingId: string,
    reason: string,
    hoursBeforeAppointment: number
  },

  // Completion
  "booking_checked_in": {
    bookingId: string
  },
  "booking_completed": {
    bookingId: string,
    actualDuration: number,
    tipAmount?: number
  },
  "booking_no_show": {
    bookingId: string
  }
}
```

#### Gamification Events
```typescript
{
  // Points & Levels
  "points_earned": {
    amount: number,
    source: "booking" | "referral" | "challenge" | "reward",
    sourceId: string,
    multiplier: number
  },
  "level_up": {
    newLevel: number,
    totalPoints: number
  },

  // Streaks
  "streak_increased": {
    newStreak: number,
    multiplier: number
  },
  "streak_broken": {
    previousStreak: number,
    reason: "missed_week" | "cancelled"
  },

  // Achievements
  "achievement_unlocked": {
    achievementId: string,
    achievementName: string,
    tier: string,
    pointsAwarded: number
  },
  "achievement_progress": {
    achievementId: string,
    progress: number,
    target: number,
    percentComplete: number
  },

  // Challenges
  "challenge_joined": {
    challengeId: string,
    challengeName: string,
    type: string
  },
  "challenge_completed": {
    challengeId: string,
    rank: number,
    score: number
  },

  // Rewards
  "reward_wheel_spun": {
    bookingId: string,
    prizeType: string,
    prizeValue: number,
    rarity: string
  },
  "reward_redeemed": {
    rewardId: string,
    prizeType: string,
    bookingId?: string
  },
  "reward_expired": {
    rewardId: string,
    prizeType: string
  }
}
```

#### Social Events
```typescript
{
  // Feed
  "feed_viewed": {
    source: "home" | "barber_profile" | "shop_profile"
  },
  "post_viewed": {
    postId: string,
    barberId: string,
    position: number, // Position in feed
    viewDuration: number // ms
  },
  "post_liked": {
    postId: string,
    barberId: string
  },
  "post_unliked": {
    postId: string
  },
  "post_saved": {
    postId: string,
    barberId: string
  },
  "post_shared": {
    postId: string,
    platform: "instagram" | "messages" | "other"
  },
  "post_commented": {
    postId: string,
    commentLength: number
  },

  // Booking from Post
  "post_booking_initiated": {
    postId: string,
    barberId: string,
    serviceIds: string[]
  },

  // Content Creation
  "post_created": {
    barberId: string,
    mediaCount: number,
    mediaTypes: string[],
    tags: string[]
  },
  "post_deleted": {
    postId: string,
    reason?: string
  }
}
```

#### VIP / Black Card Events
```typescript
{
  "black_card_viewed": {
    source: "settings" | "booking" | "promotion"
  },
  "black_card_subscribe_initiated": {
    plan: "monthly" | "yearly"
  },
  "black_card_subscribed": {
    plan: "monthly" | "yearly",
    amount: number,
    paymentMethod: string
  },
  "black_card_cancelled": {
    reason: string,
    hadForDays: number
  },
  "black_card_feature_used": {
    feature: "priority_booking" | "lounge_access" | "exclusive_service"
  },
  "lounge_access_scanned": {
    shopId: string
  }
}
```

#### Squad Events
```typescript
{
  "squad_created": {
    squadId: string,
    squadName: string,
    isPublic: boolean
  },
  "squad_joined": {
    squadId: string,
    inviteCode?: string
  },
  "squad_left": {
    squadId: string,
    memberForDays: number
  },
  "squad_challenge_created": {
    squadId: string,
    challengeType: string
  },
  "squad_member_invited": {
    squadId: string
  }
}
```

#### DJ Mode Events
```typescript
{
  "dj_mode_opened": {
    shopId: string
  },
  "dj_track_added": {
    shopId: string,
    trackId: string,
    trackName: string
  },
  "dj_vote_cast": {
    shopId: string,
    trackId: string,
    vote: "up" | "down"
  },
  "dj_track_played": {
    shopId: string,
    trackId: string,
    votes: number
  }
}
```

### iOS Implementation

#### Analytics Service
```swift
import Firebase
import Mixpanel

class AnalyticsService {
    static let shared = AnalyticsService()

    private init() {}

    func track(
        _ event: String,
        properties: [String: Any] = [:]
    ) {
        // Add common properties
        var enrichedProperties = properties
        enrichedProperties["platform"] = "ios"
        enrichedProperties["app_version"] = Bundle.main.appVersion
        enrichedProperties["user_tier"] = UserManager.shared.currentUser?.tier

        // Firebase Analytics
        Analytics.logEvent(event, parameters: enrichedProperties)

        // Mixpanel
        Mixpanel.mainInstance().track(
            event: event,
            properties: enrichedProperties
        )

        // Debug logging
        #if DEBUG
        print("[Analytics] \(event): \(enrichedProperties)")
        #endif
    }

    func identify(userId: String) {
        // Firebase
        Analytics.setUserID(userId)

        // Mixpanel
        Mixpanel.mainInstance().identify(distinctId: userId)
    }

    func setUserProperties(_ properties: [String: Any]) {
        // Firebase
        properties.forEach { key, value in
            Analytics.setUserProperty("\(value)", forName: key)
        }

        // Mixpanel
        Mixpanel.mainInstance().people.set(properties: properties)
    }
}
```

#### Tracking Usage
```swift
// Track booking creation
func createBooking(_ booking: Booking) async throws {
    let startTime = Date()

    // Create booking
    let created = try await api.createBooking(booking)

    // Track event
    AnalyticsService.shared.track("booking_created", properties: [
        "booking_id": created.id,
        "barber_id": booking.barberId,
        "shop_id": booking.shopId,
        "total_amount": booking.totalAmount,
        "is_black_card": booking.isBlackCard,
        "service_ids": booking.serviceIds,
        "time_to_complete": Date().timeIntervalSince(startTime) * 1000
    ])

    return created
}

// Track screen views
struct BookingView: View {
    var body: some View {
        // ... view content
        .onAppear {
            AnalyticsService.shared.track("screen_viewed", properties: [
                "screen_name": "booking",
                "barber_id": barberId
            ])
        }
    }
}

// Track button clicks
Button("Book Now") {
    AnalyticsService.shared.track("booking_started", properties: [
        "barber_id": barber.id,
        "source": "barber_profile"
    ])

    startBooking()
}
```

### Backend Event Tracking

```typescript
import Mixpanel from 'mixpanel';

const mixpanel = Mixpanel.init(process.env.MIXPANEL_TOKEN);

class AnalyticsService {
  track(userId: string, event: string, properties: object = {}) {
    // Add server-side properties
    const enrichedProperties = {
      ...properties,
      platform: 'backend',
      timestamp: new Date().toISOString(),
      environment: process.env.NODE_ENV
    };

    // Send to Mixpanel
    mixpanel.track(event, {
      distinct_id: userId,
      ...enrichedProperties
    });
  }

  // Track revenue
  trackRevenue(userId: string, amount: number, properties: object = {}) {
    mixpanel.people.track_charge(userId, amount, {
      ...properties,
      time: new Date()
    });
  }
}

// Usage in booking service
async function completeBooking(bookingId: string) {
  const booking = await db.bookings.findById(bookingId);

  await db.bookings.update(bookingId, { status: 'completed' });

  // Track completion
  analytics.track(booking.userId, 'booking_completed', {
    booking_id: bookingId,
    barber_id: booking.barberId,
    shop_id: booking.shopId,
    actual_duration: booking.actualDuration
  });

  // Track revenue
  analytics.trackRevenue(booking.userId, booking.totalAmount, {
    booking_id: bookingId,
    payment_method: booking.paymentMethod
  });

  // Award points
  const points = calculatePoints(booking);
  await awardPoints(booking.userId, points, 'booking_completed', bookingId);
}
```

## Key Metrics & KPIs

### User Engagement Metrics

#### Daily/Monthly Active Users
```typescript
// Mixpanel Insights Query
{
  "event": "app_opened",
  "unit": "day",
  "interval": 30,
  "type": "unique"
}
```

**Targets**:
- DAU/MAU Ratio: >30% (stickiness)
- Week 1 Retention: >40%
- Week 4 Retention: >20%
- Week 12 Retention: >10%

#### Session Metrics
```swift
class SessionTracker {
    private var sessionStart: Date?

    func startSession() {
        sessionStart = Date()

        AnalyticsService.shared.track("session_started")
    }

    func endSession() {
        guard let start = sessionStart else { return }

        let duration = Date().timeIntervalSince(start)

        AnalyticsService.shared.track("session_ended", properties: [
            "duration_seconds": duration
        ])

        sessionStart = nil
    }
}
```

**Targets**:
- Avg Session Duration: >5 minutes
- Sessions per User per Day: >2

### Booking Metrics

#### Conversion Funnel
```typescript
// Mixpanel Funnel
{
  steps: [
    { event: "shop_viewed" },
    { event: "barber_viewed" },
    { event: "booking_started" },
    { event: "booking_time_selected" },
    { event: "booking_payment_initiated" },
    { event: "booking_created" }
  ]
}
```

**Targets**:
- Shop View → Booking Started: >30%
- Booking Started → Created: >70%
- Overall Conversion: >20%

#### Booking Health
```sql
-- SQL queries for booking metrics
SELECT
  COUNT(*) as total_bookings,
  COUNT(CASE WHEN status = 'completed' THEN 1 END) as completed,
  COUNT(CASE WHEN status = 'cancelled' THEN 1 END) as cancelled,
  COUNT(CASE WHEN status = 'no-show' THEN 1 END) as no_shows,
  AVG(total_amount) as avg_booking_value
FROM bookings
WHERE created_at >= CURRENT_DATE - INTERVAL '30 days';
```

**Targets**:
- Completion Rate: >85%
- No-Show Rate: <5%
- Cancellation Rate: <10%
- Average Booking Value: $75+

### Gamification Metrics

#### Engagement Tracking
```typescript
// Track gamification effectiveness
{
  "points_per_user": "AVG(points)",
  "achievement_unlock_rate": "COUNT(achievements_unlocked) / COUNT(users)",
  "streak_participation": "COUNT(users WHERE current_streak > 0) / COUNT(users)",
  "reward_redemption_rate": "COUNT(rewards_redeemed) / COUNT(rewards_earned)"
}
```

**Targets**:
- Active Streak Users: >30%
- Achievement Unlock Rate: >5 per user
- Reward Redemption: >60%
- Challenge Participation: >20%

#### Retention Impact
```typescript
// Cohort analysis: Gamification vs Non-gamification
const cohorts = await mixpanel.query({
  from_date: '2025-01-01',
  to_date: '2025-12-31',
  event: 'booking_created',
  type: 'retention',
  retention_type: 'birth',
  born_event: 'signup_completed',
  where: 'properties["has_active_streak"] == true'
});
```

**Hypothesis**: Users with active streaks have 2x retention

### Revenue Metrics

#### Monthly Recurring Revenue (MRR)
```sql
SELECT
  SUM(CASE WHEN plan = 'monthly' THEN amount ELSE 0 END) as monthly_mrr,
  SUM(CASE WHEN plan = 'yearly' THEN amount / 12 ELSE 0 END) as yearly_mrr,
  COUNT(DISTINCT user_id) as subscribers,
  AVG(amount) as arpu
FROM subscriptions
WHERE status = 'active'
  AND created_at <= CURRENT_DATE;
```

**Targets**:
- Black Card Conversion: >5% of active users
- MRR Growth: >20% month-over-month
- Churn Rate: <5% monthly

#### Lifetime Value (LTV)
```typescript
// Calculate LTV
const avgBookingsPerMonth = 2.5;
const avgBookingValue = 75;
const avgCustomerLifetimeMonths = 24;

const ltv = avgBookingsPerMonth * avgBookingValue * avgCustomerLifetimeMonths;
// LTV = $4,500

// LTV:CAC Ratio
const cac = 50; // Customer Acquisition Cost
const ltvCacRatio = ltv / cac; // 90:1 (excellent)
```

**Targets**:
- LTV: >$3,000
- LTV:CAC Ratio: >3:1

### Social Metrics

#### Content Performance
```sql
SELECT
  COUNT(*) as total_posts,
  AVG(likes) as avg_likes,
  AVG(comments) as avg_comments,
  AVG(booking_count) as avg_bookings_per_post,
  SUM(booking_count) as total_bookings_from_posts
FROM posts
WHERE created_at >= CURRENT_DATE - INTERVAL '30 days';
```

**Targets**:
- Posts per Barber per Week: >3
- Avg Engagement Rate: >5%
- Booking Conversion from Posts: >10%

## Error Tracking & Monitoring

### Sentry Configuration

#### iOS Setup
```swift
import Sentry

class AppDelegate: UIApplicationDelegate {
    func application(
        _ application: UIApplication,
        didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?
    ) -> Bool {
        SentrySDK.start { options in
            options.dsn = "https://YOUR_DSN@sentry.io/PROJECT_ID"
            options.environment = BuildConfiguration.current.rawValue
            options.debug = BuildConfiguration.current == .debug

            // Performance monitoring
            options.tracesSampleRate = 0.2 // 20% of transactions

            // Attach screenshots on errors
            options.attachScreenshot = true

            // Breadcrumbs
            options.enableAutoSessionTracking = true
            options.enableNetworkTracking = true
        }

        return true
    }
}
```

#### Error Capture
```swift
func createBooking() async throws {
    do {
        let booking = try await api.createBooking(request)
        return booking
    } catch {
        // Capture error with context
        SentrySDK.capture(error: error) { scope in
            scope.setContext(value: [
                "booking_request": request.toDictionary(),
                "user_tier": currentUser?.tier,
                "barber_id": request.barberId
            ], key: "booking_context")

            scope.setLevel(.error)
            scope.setTag(value: "booking", key: "feature")
        }

        throw error
    }
}
```

#### Backend Setup
```typescript
import * as Sentry from '@sentry/node';
import { ProfilingIntegration } from '@sentry/profiling-node';

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  environment: process.env.NODE_ENV,
  tracesSampleRate: 0.1,
  profilesSampleRate: 0.1,
  integrations: [
    new ProfilingIntegration(),
    new Sentry.Integrations.Http({ tracing: true }),
    new Sentry.Integrations.Express({ app })
  ]
});

// Express middleware
app.use(Sentry.Handlers.requestHandler());
app.use(Sentry.Handlers.tracingHandler());

// Error handler (must be last)
app.use(Sentry.Handlers.errorHandler());
```

### Performance Monitoring

#### Custom Traces
```swift
import Firebase

class PerformanceTracker {
    static func measure<T>(
        _ name: String,
        operation: () async throws -> T
    ) async rethrows -> T {
        let trace = Performance.startTrace(name: name)

        let result = try await operation()

        trace?.stop()
        return result
    }
}

// Usage
let posts = await PerformanceTracker.measure("fetch_feed") {
    try await api.fetchFeed(page: 1)
}
```

#### Network Monitoring
```swift
class NetworkLogger: EventMonitor {
    func requestDidFinish(_ request: Request) {
        let duration = request.metrics?.taskInterval.duration ?? 0

        // Track slow requests
        if duration > 1.0 {
            AnalyticsService.shared.track("slow_network_request", properties: [
                "url": request.request?.url?.absoluteString,
                "duration": duration,
                "status_code": request.response?.statusCode
            ])
        }

        // Send to Firebase Performance
        let metric = HTTPMetric(
            url: request.request!.url!,
            httpMethod: .get
        )
        metric.responseCode = request.response?.statusCode ?? 0
        metric.stop()
    }
}
```

## Dashboards & Reporting

### Key Dashboards

#### 1. Executive Dashboard
**Metrics**:
- Daily Active Users (DAU)
- Monthly Active Users (MAU)
- Total Bookings
- Revenue (MRR, total)
- Black Card Subscribers
- User Satisfaction (NPS)

#### 2. Product Dashboard
**Metrics**:
- Feature adoption rates
- Funnel conversion rates
- Retention cohorts
- Session metrics
- Error rates

#### 3. Engineering Dashboard
**Metrics**:
- API response times (p50, p95, p99)
- Error rate by endpoint
- Crash-free sessions
- Database query performance
- WebSocket connection health

#### 4. Business Dashboard
**Metrics**:
- Revenue by source
- LTV by cohort
- CAC trends
- Churn rate
- Booking completion rate

### Custom Reports

#### Weekly Active Users by Tier
```sql
SELECT
  u.tier,
  COUNT(DISTINCT e.user_id) as active_users,
  COUNT(e.event_id) as total_events,
  COUNT(e.event_id) / COUNT(DISTINCT e.user_id) as events_per_user
FROM events e
JOIN users u ON e.user_id = u.id
WHERE e.timestamp >= CURRENT_DATE - INTERVAL '7 days'
  AND e.event_name IN ('app_opened', 'screen_viewed')
GROUP BY u.tier;
```

#### Booking Funnel by Source
```sql
WITH funnel AS (
  SELECT
    source,
    COUNT(CASE WHEN event = 'shop_viewed' THEN 1 END) as step1,
    COUNT(CASE WHEN event = 'booking_started' THEN 1 END) as step2,
    COUNT(CASE WHEN event = 'booking_created' THEN 1 END) as step3
  FROM events
  WHERE timestamp >= CURRENT_DATE - INTERVAL '30 days'
  GROUP BY source
)
SELECT
  source,
  step1,
  step2,
  step3,
  ROUND(step2::numeric / NULLIF(step1, 0) * 100, 2) as step1_to_2_conversion,
  ROUND(step3::numeric / NULLIF(step2, 0) * 100, 2) as step2_to_3_conversion
FROM funnel;
```

## Privacy & Compliance

### Data Anonymization
```typescript
// Anonymize user data for analytics
function anonymizeUser(userId: string): string {
  return crypto
    .createHash('sha256')
    .update(userId + process.env.ANALYTICS_SALT)
    .digest('hex');
}

// Track event with anonymized ID
analytics.track(anonymizeUser(userId), 'event_name', properties);
```

### GDPR Compliance
```swift
class PrivacyManager {
    func handleDataDeletion(userId: String) async throws {
        // Delete from analytics
        AnalyticsService.shared.optOut()
        Mixpanel.mainInstance().reset()

        // Request deletion from backend
        try await api.deleteUserData(userId)

        // Clear local data
        UserDefaults.standard.removePersistentDomain(forName: Bundle.main.bundleIdentifier!)
    }

    func exportUserData(userId: String) async throws -> URL {
        // Request data export from backend
        let exportUrl = try await api.requestDataExport(userId)
        return exportUrl
    }
}
```

## Alerting & Incident Response

### Critical Alerts

| Alert | Threshold | Action |
|-------|-----------|--------|
| Error Rate Spike | >5% in 5 min | Page on-call engineer |
| API Response Time | p95 >1s for 10 min | Investigate scaling |
| Crash Rate | >1% sessions | Emergency rollback |
| Booking Failures | >10 in 5 min | Check payment provider |
| WebSocket Disconnects | >50% users | Check infrastructure |

### Alert Configuration
```typescript
// Datadog alert
{
  "name": "High API Error Rate",
  "query": "avg(last_5m):sum:api.errors{*} / sum:api.requests{*} > 0.05",
  "message": "@pagerduty-cutclub High error rate detected: {{value}}%",
  "tags": ["service:api", "priority:critical"],
  "options": {
    "notify_no_data": false,
    "notify_audit": true,
    "require_full_window": true,
    "thresholds": {
      "critical": 0.05,
      "warning": 0.02
    }
  }
}
```

## Next Steps

### Implementation Priorities
1. **Week 1**: Set up Firebase Analytics + Mixpanel
2. **Week 2**: Implement core event tracking (auth, bookings)
3. **Week 3**: Add gamification events
4. **Week 4**: Set up Sentry error tracking
5. **Week 5**: Create executive dashboard
6. **Week 6**: Configure alerting

### Success Metrics (First 3 Months)
- 10K registered users
- 5K monthly active users
- 50% week-1 retention
- $50K total booking volume
- 10% Black Card conversion
- <1% error rate
- 99.9% uptime

## Summary

Complete architecture documentation created:
1. System Overview & Technology Stack
2. Data Models & Database Schema
3. API Design & Endpoints
4. Security Architecture
5. Scalability & Performance
6. Real-Time Features
7. Analytics & Monitoring

All files saved to `/Users/alexsolecarretero/Public/projects/privatestudio-v2/docs/architecture/`
