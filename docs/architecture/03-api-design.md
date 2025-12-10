# The Cut Club - API Design & Endpoints

## API Architecture

### Base Architecture
- **Style**: RESTful API with GraphQL consideration for v2
- **Protocol**: HTTPS only (TLS 1.3)
- **Format**: JSON
- **Versioning**: URL-based (`/v1/`, `/v2/`)
- **Authentication**: JWT Bearer tokens + Firebase Auth
- **Rate Limiting**: 100 requests/minute per user, 1000/minute for Black Card

### API Gateway
```
https://api.thecutclub.com/v1/
├── /auth          - Authentication & authorization
├── /users         - User management
├── /barbers       - Barber profiles & availability
├── /shops         - Shop information
├── /bookings      - Appointment management
├── /services      - Service catalog
├── /social        - Feed, posts, comments
├── /gamification  - Points, achievements, challenges
├── /rewards       - Slot machine, prizes
├── /vip           - Black Card features
├── /squads        - Friend groups
├── /payments      - Stripe integration
└── /notifications - Push, SMS, email
```

## Authentication Endpoints

### POST /auth/signup
Create new user account

**Request:**
```json
{
  "email": "user@example.com",
  "phoneNumber": "+14155551234",
  "firstName": "John",
  "lastName": "Doe",
  "dateOfBirth": "1995-06-15",
  "appleId": "optional_apple_id_token"
}
```

**Response:**
```json
{
  "user": {
    "id": "user_123",
    "email": "user@example.com",
    "displayName": "John Doe",
    "tier": "standard"
  },
  "tokens": {
    "accessToken": "jwt_token",
    "refreshToken": "refresh_token",
    "expiresIn": 3600
  }
}
```

### POST /auth/login
Authenticate user

**Request:**
```json
{
  "email": "user@example.com",
  "password": "hashed_password",
  "deviceToken": "apns_device_token"
}
```

### POST /auth/apple
Sign in with Apple

**Request:**
```json
{
  "identityToken": "apple_identity_token",
  "authorizationCode": "apple_auth_code",
  "fullName": {
    "givenName": "John",
    "familyName": "Doe"
  }
}
```

### POST /auth/refresh
Refresh access token

### POST /auth/logout
Invalidate tokens

## User Endpoints

### GET /users/me
Get current user profile

**Response:**
```json
{
  "id": "user_123",
  "email": "user@example.com",
  "displayName": "John Doe",
  "avatarUrl": "https://cdn.thecutclub.com/avatars/user_123.jpg",
  "tier": "black-card",
  "points": 2450,
  "level": 12,
  "currentStreak": 8,
  "longestStreak": 15,
  "totalVisits": 47,
  "achievements": ["ach_001", "ach_005"],
  "blackCardSince": "2024-01-15T00:00:00Z",
  "preferredBarberId": "barber_456"
}
```

### PATCH /users/me
Update user profile

**Request:**
```json
{
  "displayName": "Johnny D",
  "avatarUrl": "https://new-avatar.jpg",
  "preferredBarberId": "barber_789",
  "communicationPreferences": {
    "email": true,
    "sms": false,
    "push": true
  }
}
```

### GET /users/{userId}/stats
Get user statistics and gamification data

**Response:**
```json
{
  "points": 2450,
  "level": 12,
  "currentStreak": 8,
  "longestStreak": 15,
  "totalVisits": 47,
  "totalSpent": 1250.00,
  "achievementsUnlocked": 12,
  "totalAchievements": 50,
  "squadCount": 2,
  "friendCount": 23,
  "leaderboardRank": 145,
  "nextLevelAt": 2500,
  "streakMultiplier": 1.6
}
```

### GET /users/{userId}/achievements
Get user's achievements

**Query Params:**
- `status`: `unlocked` | `locked` | `all`
- `limit`: number (default 20)
- `offset`: number (default 0)

### GET /users/{userId}/rewards
Get user's available rewards

## Barber Endpoints

### GET /barbers
List all barbers

**Query Params:**
- `shopId`: Filter by shop
- `specialties`: Filter by specialty (fade, beard, etc.)
- `minRating`: Minimum rating (0-5)
- `availableOn`: Date (YYYY-MM-DD)
- `limit`, `offset`: Pagination

**Response:**
```json
{
  "barbers": [
    {
      "id": "barber_456",
      "displayName": "Mike The Barber",
      "avatarUrl": "https://cdn.thecutclub.com/barbers/456.jpg",
      "shopId": "shop_789",
      "shopName": "The Cut Club Downtown",
      "rating": 4.8,
      "totalReviews": 342,
      "specialties": ["fades", "designs"],
      "instagramHandle": "mikethecutgod",
      "isAcceptingNewClients": true,
      "nextAvailable": "2025-12-10T14:00:00Z"
    }
  ],
  "total": 45,
  "page": 1,
  "pageSize": 20
}
```

### GET /barbers/{barberId}
Get barber details

### GET /barbers/{barberId}/availability
Get barber's available time slots

**Query Params:**
- `date`: Date (YYYY-MM-DD)
- `duration`: Service duration in minutes
- `isBlackCard`: Priority booking flag

**Response:**
```json
{
  "date": "2025-12-10",
  "barberId": "barber_456",
  "slots": [
    {
      "time": "09:00",
      "available": true,
      "isBlackCardOnly": false
    },
    {
      "time": "09:30",
      "available": true,
      "isBlackCardOnly": false
    },
    {
      "time": "10:00",
      "available": false,
      "nextAvailableAt": "10:30"
    }
  ]
}
```

### GET /barbers/{barberId}/posts
Get barber's portfolio posts

### GET /barbers/{barberId}/reviews
Get barber reviews

## Shop Endpoints

### GET /shops
List all shops

**Query Params:**
- `latitude`, `longitude`: Geo coordinates
- `radius`: Search radius in miles (default 25)
- `amenities`: Filter by amenities
- `hasBlackCardLounge`: Boolean

**Response:**
```json
{
  "shops": [
    {
      "id": "shop_789",
      "name": "The Cut Club Downtown",
      "address": {
        "street": "123 Main St",
        "city": "San Francisco",
        "state": "CA",
        "zipCode": "94102"
      },
      "distance": 1.2,
      "rating": 4.7,
      "currentQueueLength": 3,
      "avgWaitTime": 25,
      "hasBlackCardLounge": true,
      "amenities": ["wifi", "drinks", "parking"],
      "nextAvailable": "2025-12-10T11:00:00Z"
    }
  ]
}
```

### GET /shops/{shopId}
Get shop details

### GET /shops/{shopId}/queue
Get real-time queue status

**Response:**
```json
{
  "shopId": "shop_789",
  "queueLength": 3,
  "avgWaitTime": 25,
  "currentlyServing": 4,
  "estimatedWaitTimes": {
    "standard": 25,
    "blackCard": 10
  },
  "updatedAt": "2025-12-09T14:30:00Z"
}
```

## Booking Endpoints

### POST /bookings
Create new booking

**Request:**
```json
{
  "barberId": "barber_456",
  "shopId": "shop_789",
  "serviceIds": ["service_001", "service_002"],
  "scheduledDate": "2025-12-10",
  "scheduledTime": "14:30",
  "isBlackCard": true,
  "notes": "Please do a low fade"
}
```

**Response:**
```json
{
  "booking": {
    "id": "booking_999",
    "confirmationCode": "ABC123",
    "status": "confirmed",
    "barberId": "barber_456",
    "shopId": "shop_789",
    "scheduledDate": "2025-12-10",
    "scheduledTime": "14:30",
    "duration": 60,
    "totalAmount": 85.00,
    "services": [
      {
        "id": "service_001",
        "name": "Premium Haircut",
        "price": 65.00
      },
      {
        "id": "service_002",
        "name": "Beard Trim",
        "price": 20.00
      }
    ],
    "queuePosition": 1,
    "estimatedStartTime": "2025-12-10T14:30:00Z"
  }
}
```

### GET /bookings
Get user's bookings

**Query Params:**
- `status`: Filter by status
- `upcoming`: Boolean (only future bookings)
- `limit`, `offset`: Pagination

### GET /bookings/{bookingId}
Get booking details

### PATCH /bookings/{bookingId}
Update booking (reschedule)

**Request:**
```json
{
  "scheduledDate": "2025-12-11",
  "scheduledTime": "15:00"
}
```

### DELETE /bookings/{bookingId}
Cancel booking

**Request:**
```json
{
  "cancellationReason": "Schedule conflict"
}
```

### POST /bookings/{bookingId}/checkin
Check in for appointment

### POST /bookings/{bookingId}/complete
Mark booking as completed (barber only)

## Service Endpoints

### GET /services
List all services

**Query Params:**
- `shopId`: Filter by shop
- `category`: Filter by category
- `isBlackCardExclusive`: Boolean

**Response:**
```json
{
  "services": [
    {
      "id": "service_001",
      "name": "Premium Haircut",
      "description": "Full haircut with consultation",
      "category": "haircut",
      "basePrice": 75.00,
      "blackCardPrice": 65.00,
      "duration": 45,
      "imageUrl": "https://cdn.thecutclub.com/services/haircut.jpg"
    }
  ]
}
```

## Social Feed Endpoints

### GET /social/feed
Get personalized social feed

**Query Params:**
- `limit`: Number of posts (default 20)
- `offset`: Pagination offset
- `shopIds`: Filter by specific shops
- `barberIds`: Filter by specific barbers
- `tags`: Filter by tags

**Response:**
```json
{
  "posts": [
    {
      "id": "post_001",
      "barberId": "barber_456",
      "barber": {
        "displayName": "Mike The Barber",
        "avatarUrl": "https://cdn.thecutclub.com/barbers/456.jpg"
      },
      "caption": "Fresh fade for the weekend 🔥",
      "mediaUrls": [
        {
          "type": "photo",
          "url": "https://cdn.thecutclub.com/posts/001_1.jpg",
          "thumbnailUrl": "https://cdn.thecutclub.com/posts/001_1_thumb.jpg"
        }
      ],
      "tags": ["fade", "fresh", "weekend"],
      "likes": 234,
      "comments": 18,
      "bookingCount": 12,
      "isLiked": false,
      "isSaved": true,
      "createdAt": "2025-12-09T10:30:00Z",
      "serviceIds": ["service_001"]
    }
  ],
  "hasMore": true,
  "nextOffset": 20
}
```

### POST /social/posts
Create new post (barbers only)

**Request:**
```json
{
  "caption": "Fresh fade for the weekend",
  "mediaUrls": [
    {
      "type": "photo",
      "url": "uploaded_photo_url"
    }
  ],
  "tags": ["fade", "fresh"],
  "serviceIds": ["service_001"]
}
```

### POST /social/posts/{postId}/like
Like a post

### DELETE /social/posts/{postId}/like
Unlike a post

### POST /social/posts/{postId}/save
Save post to favorites

### POST /social/posts/{postId}/book
Book from post ("I want THIS")

**Request:**
```json
{
  "preferredDate": "2025-12-15",
  "preferredTime": "afternoon",
  "notes": "Want exactly this style"
}
```

### GET /social/posts/{postId}/comments
Get post comments

### POST /social/posts/{postId}/comments
Add comment

**Request:**
```json
{
  "text": "This is fire 🔥"
}
```

## Gamification Endpoints

### GET /gamification/achievements
Get all achievements

**Query Params:**
- `userId`: Get specific user's progress
- `category`: Filter by category
- `unlocked`: Boolean

**Response:**
```json
{
  "achievements": [
    {
      "id": "ach_001",
      "title": "First Cut",
      "description": "Complete your first booking",
      "iconUrl": "https://cdn.thecutclub.com/achievements/first_cut.png",
      "tier": "bronze",
      "pointsReward": 100,
      "progress": {
        "current": 1,
        "target": 1,
        "isUnlocked": true,
        "unlockedAt": "2025-01-15T00:00:00Z"
      }
    }
  ]
}
```

### GET /gamification/leaderboard
Get global or friend leaderboard

**Query Params:**
- `type`: `global` | `friends` | `shop` | `squad`
- `timeframe`: `weekly` | `monthly` | `all-time`
- `limit`: Number of entries (default 50)

**Response:**
```json
{
  "leaderboard": [
    {
      "rank": 1,
      "userId": "user_123",
      "displayName": "John Doe",
      "avatarUrl": "https://cdn.thecutclub.com/avatars/123.jpg",
      "points": 5420,
      "level": 24,
      "currentStreak": 12,
      "isCurrentUser": false
    }
  ],
  "currentUserRank": 145,
  "totalParticipants": 10234
}
```

### GET /gamification/challenges
Get active challenges

### GET /gamification/challenges/{challengeId}
Get challenge details

### POST /gamification/challenges/{challengeId}/join
Join a challenge

### GET /gamification/challenges/{challengeId}/leaderboard
Get challenge leaderboard

## Rewards Endpoints

### POST /rewards/spin
Spin the post-visit reward wheel

**Request:**
```json
{
  "bookingId": "booking_999"
}
```

**Response:**
```json
{
  "reward": {
    "id": "reward_555",
    "type": "spin-wheel",
    "prizeType": "discount",
    "prizeName": "50% Off Next Haircut",
    "prizeValue": 37.50,
    "rarity": "rare",
    "expiresAt": "2026-01-09T23:59:59Z",
    "isRedeemed": false
  },
  "animation": {
    "landingPosition": 245,
    "spinDuration": 3000
  }
}
```

### GET /rewards
Get user's available rewards

**Query Params:**
- `isRedeemed`: Boolean
- `isExpired`: Boolean

### POST /rewards/{rewardId}/redeem
Redeem a reward

**Request:**
```json
{
  "bookingId": "booking_1000"
}
```

## VIP / Black Card Endpoints

### GET /vip/benefits
Get Black Card benefits info

**Response:**
```json
{
  "tier": "black-card",
  "benefits": [
    {
      "name": "Priority Booking",
      "description": "Skip the line with priority scheduling",
      "iconUrl": "https://cdn.thecutclub.com/benefits/priority.png"
    },
    {
      "name": "Exclusive Lounge",
      "description": "Access to private waiting lounge",
      "iconUrl": "https://cdn.thecutclub.com/benefits/lounge.png"
    }
  ],
  "pricing": {
    "monthly": 99.00,
    "yearly": 999.00,
    "savings": 189.00
  }
}
```

### POST /vip/subscribe
Subscribe to Black Card

**Request:**
```json
{
  "plan": "monthly" | "yearly",
  "paymentMethodId": "stripe_pm_id"
}
```

### POST /vip/cancel
Cancel Black Card subscription

### GET /vip/lounge/access
Get lounge access QR code

**Response:**
```json
{
  "accessCode": "BC-123456",
  "qrCodeUrl": "https://api.thecutclub.com/vip/qr/BC-123456",
  "validUntil": "2025-12-09T23:59:59Z"
}
```

## Squad Endpoints

### GET /squads
Get user's squads

### POST /squads
Create new squad

**Request:**
```json
{
  "name": "The Faded Crew",
  "isPublic": false,
  "maxMembers": 10
}
```

### GET /squads/{squadId}
Get squad details

### POST /squads/{squadId}/join
Join squad (with invite code if private)

### DELETE /squads/{squadId}/leave
Leave squad

### GET /squads/{squadId}/members
Get squad members

### GET /squads/{squadId}/activity
Get squad activity feed

### POST /squads/{squadId}/challenge
Challenge squad to competition

**Request:**
```json
{
  "challengeType": "freshest_fade",
  "endDate": "2025-12-31"
}
```

## Payment Endpoints

### POST /payments/methods
Add payment method

**Request:**
```json
{
  "stripePaymentMethodId": "pm_xxxxxxxxx",
  "isDefault": true
}
```

### GET /payments/methods
Get saved payment methods

### DELETE /payments/methods/{methodId}
Remove payment method

### POST /payments/intents
Create payment intent for booking

**Request:**
```json
{
  "bookingId": "booking_999",
  "amount": 85.00,
  "tipAmount": 15.00
}
```

**Response:**
```json
{
  "clientSecret": "pi_xxxxx_secret_yyyyy",
  "paymentIntentId": "pi_xxxxx",
  "amount": 100.00,
  "currency": "usd"
}
```

### POST /payments/tips
Add tip after service

**Request:**
```json
{
  "bookingId": "booking_999",
  "amount": 15.00
}
```

## Notification Endpoints

### GET /notifications
Get user notifications

**Query Params:**
- `isRead`: Filter by read status
- `types`: Filter by notification types
- `limit`, `offset`: Pagination

### PATCH /notifications/{notificationId}/read
Mark notification as read

### POST /notifications/mark-all-read
Mark all as read

### POST /notifications/preferences
Update notification preferences

**Request:**
```json
{
  "bookingReminders": {
    "push": true,
    "email": true,
    "sms": false,
    "advanceTime": 24
  },
  "socialInteractions": {
    "push": true,
    "email": false
  },
  "promotions": {
    "push": false,
    "email": true
  }
}
```

## WebSocket Events (Real-Time)

### Connection
```javascript
wss://api.thecutclub.com/v1/ws?token=jwt_token
```

### Events to Subscribe

#### Queue Updates
```json
{
  "event": "subscribe",
  "channel": "shop:queue:shop_789"
}

// Server sends:
{
  "event": "queue:update",
  "data": {
    "shopId": "shop_789",
    "queueLength": 4,
    "avgWaitTime": 30,
    "yourPosition": 2
  }
}
```

#### Booking Status
```json
{
  "event": "subscribe",
  "channel": "booking:booking_999"
}

// Server sends:
{
  "event": "booking:status_change",
  "data": {
    "bookingId": "booking_999",
    "status": "in-progress",
    "estimatedCompletion": "2025-12-09T15:30:00Z"
  }
}
```

#### Social Feed (Live)
```json
{
  "event": "subscribe",
  "channel": "social:feed"
}

// Server sends:
{
  "event": "post:new",
  "data": {
    "postId": "post_123",
    "barberId": "barber_456",
    "preview": "..."
  }
}
```

#### DJ Mode Voting
```json
{
  "event": "subscribe",
  "channel": "shop:dj:shop_789"
}

// Client sends vote:
{
  "event": "dj:vote",
  "data": {
    "songId": "spotify_track_id",
    "vote": "up" | "down"
  }
}

// Server broadcasts:
{
  "event": "dj:vote_update",
  "data": {
    "songId": "spotify_track_id",
    "upvotes": 12,
    "downvotes": 3,
    "position": 2
  }
}
```

## Error Responses

### Standard Error Format
```json
{
  "error": {
    "code": "BOOKING_CONFLICT",
    "message": "This time slot is no longer available",
    "details": {
      "barberId": "barber_456",
      "requestedTime": "14:30",
      "nextAvailable": "15:00"
    },
    "timestamp": "2025-12-09T14:35:00Z"
  }
}
```

### Error Codes
- `UNAUTHORIZED` (401)
- `FORBIDDEN` (403)
- `NOT_FOUND` (404)
- `BOOKING_CONFLICT` (409)
- `RATE_LIMIT_EXCEEDED` (429)
- `VALIDATION_ERROR` (400)
- `PAYMENT_FAILED` (402)
- `SERVER_ERROR` (500)

## Rate Limiting

### Headers
```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1638360000
```

### Limits
- **Standard Users**: 100 req/min
- **Black Card**: 1000 req/min
- **Barbers**: 500 req/min
- **WebSocket**: 100 messages/min

## Pagination

### Request
```
GET /social/feed?limit=20&offset=40
```

### Response
```json
{
  "data": [...],
  "pagination": {
    "total": 1234,
    "limit": 20,
    "offset": 40,
    "hasMore": true
  }
}
```

## Next: Technology Stack Details
See `/docs/architecture/04-technology-stack.md` for comprehensive tech evaluation.
