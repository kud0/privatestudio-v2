# The Cut Club - Data Models & Database Schema

## Database Strategy

### Firebase Firestore Collections Structure

```
/users/{userId}
/barbers/{barberId}
/shops/{shopId}
/bookings/{bookingId}
/services/{serviceId}
/posts/{postId}
/rewards/{rewardId}
/achievements/{achievementId}
/challenges/{challengeId}
/squads/{squadId}
/notifications/{notificationId}
/reviews/{reviewId}
/transactions/{transactionId}
```

## Core Data Models

### 1. User Model

```typescript
interface User {
  // Identity
  id: string;                      // Firebase Auth UID
  email: string;
  phoneNumber: string;
  appleId?: string;                // Sign in with Apple

  // Profile
  firstName: string;
  lastName: string;
  displayName: string;
  avatarUrl?: string;
  dateOfBirth: Date;

  // Preferences
  preferredBarberId?: string;
  favoriteStyles: string[];        // Array of post IDs
  communicationPreferences: {
    email: boolean;
    sms: boolean;
    push: boolean;
  };

  // Gamification
  points: number;                  // Total points earned
  level: number;                   // User level (1-100)
  currentStreak: number;           // Current visit streak
  longestStreak: number;           // Best streak ever
  lastVisitDate?: Date;
  totalVisits: number;
  achievementIds: string[];        // Unlocked achievements

  // VIP Status
  tier: 'standard' | 'premium' | 'black-card';
  blackCardSince?: Date;
  blackCardExpires?: Date;

  // Social
  following: string[];             // User IDs
  followers: string[];             // User IDs
  squadIds: string[];              // Squad memberships

  // Metadata
  createdAt: Date;
  updatedAt: Date;
  lastActiveAt: Date;
  isActive: boolean;
  isVerified: boolean;
}
```

### 2. Barber Model

```typescript
interface Barber {
  // Identity
  id: string;
  userId: string;                  // Reference to User
  shopId: string;                  // Primary shop

  // Profile
  displayName: string;
  bio: string;
  specialties: string[];           // ['fades', 'beards', 'designs']
  avatarUrl: string;

  // Ratings
  rating: number;                  // 0-5 stars
  totalReviews: number;
  totalBookings: number;

  // Schedule
  workingHours: {
    [day: string]: {               // 'monday', 'tuesday', etc.
      isWorking: boolean;
      start: string;               // '09:00'
      end: string;                 // '18:00'
      breaks: Array<{
        start: string;
        end: string;
      }>;
    };
  };
  offDates: Date[];                // Vacation/time off

  // Social
  instagramHandle?: string;
  postIds: string[];               // Portfolio posts
  followerIds: string[];

  // Performance
  completionRate: number;          // % of bookings completed
  avgServiceTime: number;          // Minutes per service
  repeatClientRate: number;        // % of returning clients

  // Metadata
  joinedAt: Date;
  isActive: boolean;
  isAcceptingNewClients: boolean;
}
```

### 3. Shop Model

```typescript
interface Shop {
  // Identity
  id: string;
  name: string;

  // Location
  address: {
    street: string;
    city: string;
    state: string;
    zipCode: string;
    country: string;
    coordinates: {
      latitude: number;
      longitude: number;
    };
  };

  // Contact
  phoneNumber: string;
  email: string;
  website?: string;
  instagramHandle?: string;

  // Hours
  hours: {
    [day: string]: {
      isOpen: boolean;
      open: string;
      close: string;
    };
  };
  holidays: Date[];

  // Amenities
  amenities: string[];             // ['wifi', 'drinks', 'parking', 'lounge']
  hasBlackCardLounge: boolean;

  // Staff
  barberIds: string[];
  managerId: string;

  // Media
  photos: string[];
  logoUrl: string;

  // Queue Management
  currentQueueLength: number;
  avgWaitTime: number;             // Minutes

  // Metadata
  createdAt: Date;
  isActive: boolean;
}
```

### 4. Booking Model

```typescript
interface Booking {
  // Identity
  id: string;

  // Relations
  userId: string;
  barberId: string;
  shopId: string;
  serviceIds: string[];

  // Schedule
  scheduledDate: Date;
  scheduledTime: string;           // '14:30'
  duration: number;                // Minutes

  // Status
  status: 'pending' | 'confirmed' | 'in-progress' | 'completed' | 'cancelled' | 'no-show';
  confirmationCode: string;        // 6-digit code

  // Priority
  isBlackCard: boolean;            // Skip-the-line priority
  isPriorityBooking: boolean;
  queuePosition?: number;

  // Payment
  totalAmount: number;
  tipAmount?: number;
  paymentStatus: 'pending' | 'paid' | 'refunded';
  paymentIntentId?: string;        // Stripe

  // Communication
  reminderSentAt?: Date;

  // Post-Visit
  hasSpunWheel: boolean;           // Slot machine reward claimed
  reviewId?: string;

  // Metadata
  createdAt: Date;
  updatedAt: Date;
  cancelledAt?: Date;
  cancelledBy?: string;            // userId or 'system'
  cancellationReason?: string;

  // Streak Tracking
  countsTowardStreak: boolean;
  streakWeek: number;              // Which week of streak
}
```

### 5. Service Model

```typescript
interface Service {
  id: string;
  shopId: string;

  // Details
  name: string;                    // 'Classic Haircut', 'Beard Trim'
  description: string;
  category: 'haircut' | 'beard' | 'styling' | 'treatment' | 'premium';

  // Pricing
  basePrice: number;
  blackCardPrice?: number;         // Discounted for VIP
  duration: number;                // Minutes

  // Availability
  isActive: boolean;
  isBlackCardExclusive: boolean;

  // Media
  imageUrl?: string;

  // Metadata
  createdAt: Date;
  updatedAt: Date;
}
```

### 6. Post Model (Social Feed)

```typescript
interface Post {
  // Identity
  id: string;

  // Author
  barberId: string;
  shopId: string;

  // Content
  caption: string;
  mediaUrls: Array<{
    type: 'photo' | 'video';
    url: string;
    thumbnailUrl?: string;
  }>;
  tags: string[];                  // ['fade', 'lineup', 'beard']

  // Engagement
  likes: number;
  likedBy: string[];               // User IDs (limit to 100, rest in subcollection)
  comments: number;                // Count (actual comments in subcollection)
  shares: number;
  saves: number;                   // Users who saved to favorites

  // Booking Integration
  bookingCount: number;            // "I want THIS" bookings
  serviceIds: string[];            // Related services

  // Visibility
  isPublic: boolean;
  isFeatured: boolean;             // Highlighted in app

  // Metadata
  createdAt: Date;
  updatedAt: Date;
}

// Subcollection: /posts/{postId}/comments/{commentId}
interface Comment {
  id: string;
  userId: string;
  text: string;
  likes: number;
  createdAt: Date;
  isDeleted: boolean;
}
```

### 7. Reward Model

```typescript
interface Reward {
  // Identity
  id: string;

  // Type
  type: 'spin-wheel' | 'achievement' | 'streak-bonus' | 'referral' | 'challenge';

  // Prize
  prizeType: 'discount' | 'free-service' | 'merchandise' | 'vip-upgrade' | 'points';
  prizeName: string;               // 'Free Beard Trim', '50% Off Next Cut'
  prizeValue: number;              // Dollar amount or point value

  // Redemption
  userId: string;
  isRedeemed: boolean;
  redeemedAt?: Date;
  expiresAt?: Date;

  // Source
  sourceType: string;              // 'post_visit_spin', 'achievement_unlock'
  sourceId?: string;               // booking ID, achievement ID

  // Rarity
  rarity: 'common' | 'rare' | 'epic' | 'legendary';
  dropRate: number;                // Probability (0-1)

  // Metadata
  createdAt: Date;
}
```

### 8. Achievement Model

```typescript
interface Achievement {
  // Identity
  id: string;

  // Display
  title: string;                   // 'First Cut', 'Streak Master'
  description: string;
  iconUrl: string;
  badgeUrl: string;                // Collectible badge image

  // Rarity
  tier: 'bronze' | 'silver' | 'gold' | 'platinum' | 'diamond';
  rarity: number;                  // % of users who have it

  // Unlock Criteria
  criteria: {
    type: 'visits' | 'streak' | 'referrals' | 'social' | 'spending';
    threshold: number;
    description: string;           // 'Complete 10 visits'
  };

  // Rewards
  pointsReward: number;
  unlocksReward?: string;          // Reward ID for special prize

  // Visibility
  isSecret: boolean;               // Hidden until unlocked
  isActive: boolean;

  // Metadata
  createdAt: Date;
}

// User progress subcollection: /users/{userId}/achievement_progress/{achievementId}
interface AchievementProgress {
  achievementId: string;
  currentProgress: number;
  targetProgress: number;
  unlockedAt?: Date;
  isCompleted: boolean;
}
```

### 9. Challenge Model

```typescript
interface Challenge {
  // Identity
  id: string;

  // Details
  title: string;                   // 'Summer Fade Challenge'
  description: string;
  imageUrl: string;

  // Timing
  startDate: Date;
  endDate: Date;
  isActive: boolean;

  // Rules
  challengeType: 'individual' | 'squad' | 'shop' | 'global';
  objective: {
    type: 'visits' | 'bookings' | 'referrals' | 'social-engagement';
    target: number;
    description: string;
  };

  // Rewards
  rewards: Array<{
    rank: number;                  // 1st, 2nd, 3rd, etc.
    prizeDescription: string;
    pointsReward: number;
    physicalReward?: string;
  }>;

  // Participation
  participantCount: number;

  // Metadata
  createdAt: Date;
}

// Leaderboard subcollection: /challenges/{challengeId}/leaderboard/{userId}
interface ChallengeLeaderboard {
  userId: string;
  score: number;
  rank: number;
  lastUpdated: Date;
}
```

### 10. Squad Model

```typescript
interface Squad {
  // Identity
  id: string;
  name: string;
  avatarUrl?: string;

  // Members
  creatorId: string;
  memberIds: string[];
  maxMembers: number;              // Default 10

  // Stats
  totalPoints: number;
  totalVisits: number;
  currentStreak: number;           // Collective streak

  // Challenges
  activeChallengeIds: string[];

  // Social
  isPublic: boolean;               // Can anyone join?
  inviteCode?: string;             // For private squads

  // Communication
  lastActivityAt: Date;

  // Metadata
  createdAt: Date;
  isActive: boolean;
}

// Squad activity subcollection: /squads/{squadId}/activity/{activityId}
interface SquadActivity {
  id: string;
  type: 'member_joined' | 'visit_completed' | 'challenge_won' | 'achievement_unlocked';
  userId: string;
  description: string;
  points?: number;
  createdAt: Date;
}
```

### 11. Notification Model

```typescript
interface Notification {
  // Identity
  id: string;
  userId: string;

  // Content
  type: 'booking_reminder' | 'booking_confirmed' | 'achievement_unlocked' |
        'reward_expiring' | 'squad_activity' | 'social_interaction' |
        'challenge_started' | 'promotion';

  title: string;
  message: string;
  imageUrl?: string;

  // Action
  actionType?: 'open_booking' | 'open_reward' | 'open_profile' | 'open_post' | 'open_challenge';
  actionId?: string;               // ID of related entity

  // Status
  isRead: boolean;
  readAt?: Date;

  // Delivery
  sentVia: Array<'push' | 'email' | 'sms'>;
  sentAt: Date;

  // Metadata
  createdAt: Date;
  expiresAt?: Date;
}
```

### 12. Review Model

```typescript
interface Review {
  // Identity
  id: string;

  // Relations
  userId: string;
  barberId: string;
  shopId: string;
  bookingId: string;

  // Rating
  rating: number;                  // 1-5 stars

  // Content
  title?: string;
  comment?: string;

  // Specific Ratings
  skillRating: number;             // 1-5
  punctualityRating: number;       // 1-5
  vibeRating: number;              // 1-5

  // Response
  barberResponse?: string;
  respondedAt?: Date;

  // Verification
  isVerifiedVisit: boolean;        // Confirmed booking

  // Engagement
  helpfulCount: number;            // Other users marked as helpful

  // Metadata
  createdAt: Date;
  updatedAt: Date;
  isPublic: boolean;
}
```

### 13. Transaction Model

```typescript
interface Transaction {
  // Identity
  id: string;

  // Relations
  userId: string;
  bookingId?: string;              // If related to booking

  // Type
  type: 'booking_payment' | 'tip' | 'product_purchase' | 'refund' | 'vip_subscription';

  // Amount
  amount: number;
  currency: 'USD';

  // Payment
  paymentMethod: 'apple_pay' | 'card' | 'points';
  stripePaymentIntentId?: string;
  stripeChargeId?: string;

  // Status
  status: 'pending' | 'succeeded' | 'failed' | 'refunded';

  // Points
  pointsEarned?: number;           // Reward points from transaction
  pointsSpent?: number;            // If paying with points

  // Metadata
  createdAt: Date;
  processedAt?: Date;
  failureReason?: string;
}
```

## Indexes & Queries

### Critical Firestore Indexes

```javascript
// User queries
users: {
  email: 'asc',
  phoneNumber: 'asc',
  tier: 'asc',
  points: 'desc'
}

// Booking queries
bookings: {
  userId_status: ['userId', 'status'],
  barberId_scheduledDate: ['barberId', 'scheduledDate'],
  shopId_scheduledDate_status: ['shopId', 'scheduledDate', 'status'],
  isBlackCard_scheduledDate: ['isBlackCard', 'scheduledDate']
}

// Post queries
posts: {
  barberId_createdAt: ['barberId', 'createdAt', 'desc'],
  shopId_createdAt: ['shopId', 'createdAt', 'desc'],
  isFeatured_createdAt: ['isFeatured', 'createdAt', 'desc'],
  tags_createdAt: ['tags', 'createdAt', 'desc']
}

// Achievement progress
achievement_progress: {
  userId_isCompleted: ['userId', 'isCompleted'],
  achievementId_currentProgress: ['achievementId', 'currentProgress', 'desc']
}

// Leaderboards
challenge_leaderboard: {
  challengeId_rank: ['challengeId', 'rank'],
  challengeId_score: ['challengeId', 'score', 'desc']
}
```

## Data Access Patterns

### Most Frequent Queries

1. **Get upcoming bookings for user**
   ```
   bookings.where('userId', '==', uid)
          .where('status', 'in', ['pending', 'confirmed'])
          .where('scheduledDate', '>=', today)
          .orderBy('scheduledDate')
   ```

2. **Get available time slots for barber**
   ```
   bookings.where('barberId', '==', barberId)
          .where('scheduledDate', '==', date)
          .where('status', '!=', 'cancelled')
   ```

3. **Get social feed**
   ```
   posts.where('shopId', 'in', nearbyShopIds)
        .orderBy('createdAt', 'desc')
        .limit(20)
   ```

4. **Get user's leaderboard position**
   ```
   challenge_leaderboard.where('challengeId', '==', id)
                        .where('userId', '==', uid)
   ```

5. **Get Black Card holder bookings (priority)**
   ```
   bookings.where('shopId', '==', shopId)
          .where('isBlackCard', '==', true)
          .where('scheduledDate', '==', date)
          .orderBy('scheduledTime')
   ```

## Caching Strategy

### Client-Side (iOS)
- **Core Data**: User profile, bookings, favorites
- **NSCache**: Images, barber profiles
- **UserDefaults**: Settings, last sync timestamp

### Server-Side (Redis)
- **Hot Data**: Active bookings (15 min TTL)
- **Leaderboards**: Challenge rankings (5 min TTL)
- **Queue Status**: Shop wait times (1 min TTL)
- **Feed**: Popular posts (10 min TTL)

## Next: API Design
See `/docs/architecture/03-api-design.md` for endpoint specifications.
