# The Cut Club - Scalability & Performance Strategy

## Performance Targets

### User Experience Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Cold Start Time | <2s | App launch to first interaction |
| Warm Start Time | <0.5s | Background to foreground |
| API Response Time (p95) | <100ms | Critical paths (bookings, feed) |
| API Response Time (p99) | <500ms | All endpoints |
| Time to First Content | <1s | Feed, bookings list |
| Image Load Time | <200ms | Thumbnails with progressive loading |
| Video Start Time | <1s | Feed videos |
| Booking Creation | <2s | End-to-end booking flow |
| Animation Frame Rate | 60fps | All transitions and gestures |

### Backend Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Availability | 99.9% | <8.76 hours downtime/year |
| Error Rate | <0.1% | Failed requests / total requests |
| Database Query Time | <50ms | p95 for indexed queries |
| WebSocket Latency | <50ms | Queue updates, live features |
| CDN Cache Hit Rate | >90% | Images and static content |

## iOS App Performance

### Launch Optimization

#### Cold Start Strategy
```swift
// AppDelegate optimization
class AppDelegate: UIResponder, UIApplicationDelegate {
    func application(
        _ application: UIApplication,
        didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?
    ) -> Bool {
        // 1. Critical initialization only (< 400ms target)
        configureFirebase() // ~100ms
        setupAnalytics()    // ~50ms
        initializeKeychain() // ~20ms

        // 2. Defer non-critical work
        DispatchQueue.main.async {
            self.configureNotifications()
            self.setupRemoteConfig()
            self.preloadCriticalData()
        }

        // 3. Background initialization
        DispatchQueue.global(qos: .utility).async {
            self.setupCrashReporting()
            self.initializeImageCache()
            self.cleanupOldData()
        }

        return true
    }
}
```

#### Warm Start Optimization
```swift
// Pre-warm critical views
class SceneDelegate: UIWindowSceneDelegate {
    func sceneWillEnterForeground(_ scene: UIScene) {
        // Refresh time-sensitive data
        Task {
            await refreshUpcomingBookings()
            await updateQueueStatus()
        }

        // Pre-load likely next screen
        preloadLikelyDestinations()
    }
}
```

### Memory Management

#### Image Caching Strategy
```swift
import Kingfisher

// Configure Kingfisher
let cache = ImageCache.default
cache.memoryStorage.config.totalCostLimit = 100 * 1024 * 1024 // 100MB
cache.diskStorage.config.sizeLimit = 500 * 1024 * 1024 // 500MB
cache.diskStorage.config.expiration = .days(7)

// Progressive image loading
func loadImage(url: URL, into imageView: UIImageView) {
    imageView.kf.setImage(
        with: url,
        placeholder: UIImage(named: "placeholder"),
        options: [
            .transition(.fade(0.2)),
            .cacheOriginalImage,
            .scaleFactor(UIScreen.main.scale),
            .processor(DownsamplingImageProcessor(size: imageView.bounds.size))
        ]
    )
}
```

#### Memory Pressure Handling
```swift
class MemoryMonitor {
    func observeMemoryWarnings() {
        NotificationCenter.default.addObserver(
            forName: UIApplication.didReceiveMemoryWarningNotification,
            object: nil,
            queue: .main
        ) { [weak self] _ in
            self?.handleMemoryWarning()
        }
    }

    private func handleMemoryWarning() {
        // Clear caches
        ImageCache.default.clearMemoryCache()

        // Remove unnecessary view controllers
        cleanupNavigationStack()

        // Cancel pending network requests
        cancelLowPriorityRequests()

        Logger.performance.warning("Memory warning received")
    }
}
```

### Network Optimization

#### Request Batching
```swift
class APIBatcher {
    private var pendingRequests: [APIRequest] = []
    private var batchTimer: Timer?

    func enqueue(_ request: APIRequest) {
        pendingRequests.append(request)

        // Batch requests within 50ms window
        batchTimer?.invalidate()
        batchTimer = Timer.scheduledTimer(
            withTimeInterval: 0.05,
            repeats: false
        ) { [weak self] _ in
            self?.executeBatch()
        }
    }

    private func executeBatch() {
        guard !pendingRequests.isEmpty else { return }

        // Send all requests in parallel
        Task {
            await withTaskGroup(of: Void.self) { group in
                for request in pendingRequests {
                    group.addTask {
                        await request.execute()
                    }
                }
            }
        }

        pendingRequests.removeAll()
    }
}
```

#### Response Caching
```swift
class CachedAPIClient {
    private let cache = URLCache(
        memoryCapacity: 50 * 1024 * 1024,  // 50MB
        diskCapacity: 100 * 1024 * 1024,   // 100MB
        diskPath: "api_cache"
    )

    func request<T: Decodable>(
        _ endpoint: Endpoint,
        cachePolicy: CachePolicy = .default
    ) async throws -> T {
        let request = createRequest(endpoint)

        // Check cache first for cacheable endpoints
        if cachePolicy.shouldReadCache,
           let cachedResponse = cache.cachedResponse(for: request),
           !isCacheExpired(cachedResponse) {
            return try JSONDecoder().decode(T.self, from: cachedResponse.data)
        }

        // Fetch from network
        let (data, response) = try await URLSession.shared.data(for: request)

        // Cache if appropriate
        if cachePolicy.shouldWriteCache, let httpResponse = response as? HTTPURLResponse {
            let cachedResponse = CachedURLResponse(
                response: httpResponse,
                data: data,
                storagePolicy: .allowed
            )
            cache.storeCachedResponse(cachedResponse, for: request)
        }

        return try JSONDecoder().decode(T.self, from: data)
    }
}
```

#### Offline Support
```swift
class OfflineManager {
    func enableOfflineMode() {
        // 1. Cache critical data
        Task {
            await cacheUserProfile()
            await cacheUpcomingBookings()
            await cacheFavoriteBarbers()
            await cacheShopLocations()
        }

        // 2. Enable Firestore persistence
        let settings = FirestoreSettings()
        settings.isPersistenceEnabled = true
        settings.cacheSizeBytes = 100 * 1024 * 1024 // 100MB
        Firestore.firestore().settings = settings

        // 3. Queue offline actions
        enableOfflineQueue()
    }

    func handleOfflineBooking(_ booking: Booking) {
        // Store in local queue
        offlineQueue.append(.createBooking(booking))

        // Show user-friendly message
        showOfflineNotice("Booking will be created when online")

        // Sync when connection restored
        observeConnectivityChanges()
    }
}
```

### List Performance

#### Efficient List Rendering
```swift
import SwiftUI

// Virtual scrolling with pagination
struct OptimizedFeedView: View {
    @StateObject private var viewModel = FeedViewModel()

    var body: some View {
        List {
            ForEach(viewModel.posts) { post in
                PostCell(post: post)
                    .onAppear {
                        // Load more when reaching bottom
                        if viewModel.shouldLoadMore(post) {
                            Task {
                                await viewModel.loadNextPage()
                            }
                        }
                    }
            }
        }
        .listStyle(.plain)
    }
}

// Optimized cell
struct PostCell: View {
    let post: Post

    var body: some View {
        VStack(alignment: .leading, spacing: 8) {
            // Use lazy loading for images
            LazyImage(url: post.imageUrl)

            Text(post.caption)
                .lineLimit(3)
        }
        .id(post.id) // Stable identity for diffing
    }
}
```

#### Data Prefetching
```swift
class FeedViewModel: ObservableObject {
    @Published var posts: [Post] = []
    private var prefetchedPages: [Int: [Post]] = [:]
    private var currentPage = 0

    func loadNextPage() async {
        currentPage += 1

        // Check if already prefetched
        if let cached = prefetchedPages[currentPage] {
            posts.append(contentsOf: cached)
            prefetchedPages[currentPage] = nil
            prefetchNext() // Continue prefetching
            return
        }

        // Fetch from network
        let newPosts = try? await api.fetchPosts(page: currentPage)
        if let newPosts {
            posts.append(contentsOf: newPosts)
            prefetchNext()
        }
    }

    private func prefetchNext() {
        Task {
            let nextPage = currentPage + 1
            guard prefetchedPages[nextPage] == nil else { return }

            let posts = try? await api.fetchPosts(page: nextPage)
            prefetchedPages[nextPage] = posts
        }
    }

    func shouldLoadMore(_ post: Post) -> Bool {
        // Trigger 5 items before end
        guard let index = posts.firstIndex(where: { $0.id == post.id }) else {
            return false
        }
        return index >= posts.count - 5
    }
}
```

## Backend Scalability

### Database Scaling

#### Firebase Firestore Strategy

**Automatic Scaling** (Built-in):
- Handles 10K+ concurrent connections
- Automatic sharding
- Multi-region replication

**Optimization Techniques**:

```typescript
// 1. Efficient queries with indexes
const bookings = await db.collection('bookings')
  .where('shopId', '==', shopId)
  .where('scheduledDate', '>=', today)
  .where('status', '==', 'confirmed')
  .orderBy('scheduledDate')
  .limit(20)
  .get();

// 2. Denormalization for read performance
interface BookingDocument {
  // Instead of just barberId, denormalize
  barber: {
    id: string;
    name: string;
    avatarUrl: string;
  };
  // Avoid separate read for barber details
}

// 3. Aggregation with subcollections
// /shops/{shopId}/stats/daily/{date}
interface DailyStats {
  date: string;
  totalBookings: number;
  totalRevenue: number;
  avgWaitTime: number;
}

// 4. Batch writes for efficiency
const batch = db.batch();
bookings.forEach(booking => {
  batch.set(db.collection('bookings').doc(booking.id), booking);
});
await batch.commit();
```

**Sharding Strategy** (for hot documents):
```typescript
// Problem: Single shop document updated frequently
// Solution: Shard by time or counter

// Instead of:
// /shops/{shopId} { queueLength: 5 }

// Use:
// /shops/{shopId}/queue_shards/{shard_0} { count: 2 }
// /shops/{shopId}/queue_shards/{shard_1} { count: 1 }
// /shops/{shopId}/queue_shards/{shard_2} { count: 2 }

async function incrementQueueLength(shopId: string) {
  const shardId = Math.floor(Math.random() * 10);
  const shardRef = db.doc(`shops/${shopId}/queue_shards/shard_${shardId}`);

  await shardRef.update({
    count: admin.firestore.FieldValue.increment(1)
  });
}

async function getQueueLength(shopId: string) {
  const shards = await db.collection(`shops/${shopId}/queue_shards`).get();
  return shards.docs.reduce((sum, doc) => sum + doc.data().count, 0);
}
```

#### PostgreSQL Scaling (Custom Backend)

**Read Replicas**:
```typescript
// Primary for writes
const primaryDB = new Pool({
  host: 'primary.db.thecutclub.com',
  database: 'cutclub',
  max: 20
});

// Replica for reads
const replicaDB = new Pool({
  host: 'replica.db.thecutclub.com',
  database: 'cutclub',
  max: 50
});

// Read from replica
async function getBookings(userId: string) {
  return replicaDB.query(
    'SELECT * FROM bookings WHERE user_id = $1',
    [userId]
  );
}

// Write to primary
async function createBooking(booking: Booking) {
  return primaryDB.query(
    'INSERT INTO bookings (...) VALUES (...)',
    [...]
  );
}
```

**Connection Pooling**:
```typescript
import { Pool } from 'pg';

const pool = new Pool({
  max: 50,              // Max connections
  idleTimeoutMillis: 30000,
  connectionTimeoutMillis: 2000,
});

// Optimize with prepared statements
const createBookingStmt = await pool.query({
  name: 'create-booking',
  text: 'INSERT INTO bookings (user_id, barber_id, ...) VALUES ($1, $2, ...)',
});
```

**Partitioning** (by date for bookings):
```sql
-- Create partitioned table
CREATE TABLE bookings (
    id UUID PRIMARY KEY,
    user_id UUID NOT NULL,
    scheduled_date DATE NOT NULL,
    ...
) PARTITION BY RANGE (scheduled_date);

-- Create partitions (automated)
CREATE TABLE bookings_2025_12 PARTITION OF bookings
    FOR VALUES FROM ('2025-12-01') TO ('2026-01-01');

CREATE TABLE bookings_2026_01 PARTITION OF bookings
    FOR VALUES FROM ('2026-01-01') TO ('2026-02-01');

-- Indexes on partitions
CREATE INDEX idx_bookings_2025_12_user ON bookings_2025_12 (user_id);
CREATE INDEX idx_bookings_2025_12_barber ON bookings_2025_12 (barber_id);
```

### Caching Strategy

#### Multi-Layer Caching

```typescript
// Layer 1: In-Memory (Application)
import NodeCache from 'node-cache';

const appCache = new NodeCache({
  stdTTL: 60,      // 60 seconds default
  checkperiod: 120  // Cleanup every 2 minutes
});

// Layer 2: Redis (Shared)
import Redis from 'ioredis';

const redis = new Redis({
  host: 'redis.thecutclub.com',
  port: 6379,
  retryStrategy: (times) => Math.min(times * 50, 2000)
});

// Layer 3: CDN (Edge)
// Cloudflare caches responses with appropriate headers

// Unified cache service
class CacheService {
  async get<T>(key: string): Promise<T | null> {
    // L1: Check app cache
    let value = appCache.get<T>(key);
    if (value) return value;

    // L2: Check Redis
    const cached = await redis.get(key);
    if (cached) {
      value = JSON.parse(cached);
      appCache.set(key, value); // Promote to L1
      return value;
    }

    return null;
  }

  async set<T>(key: string, value: T, ttl: number = 60): Promise<void> {
    // Set in both layers
    appCache.set(key, value, ttl);
    await redis.setex(key, ttl, JSON.stringify(value));
  }

  async invalidate(key: string | string[]): Promise<void> {
    const keys = Array.isArray(key) ? key : [key];

    keys.forEach(k => appCache.del(k));
    await redis.del(...keys);
  }
}
```

#### Cache Keys Design
```typescript
const CacheKeys = {
  user: (userId: string) => `user:${userId}`,
  userBookings: (userId: string) => `user:${userId}:bookings`,
  barberAvailability: (barberId: string, date: string) =>
    `barber:${barberId}:availability:${date}`,
  shopQueue: (shopId: string) => `shop:${shopId}:queue`,
  leaderboard: (type: string, timeframe: string) =>
    `leaderboard:${type}:${timeframe}`,
  feedPosts: (page: number) => `feed:posts:page:${page}`,
};
```

#### Cache Invalidation Strategy
```typescript
// Event-driven invalidation
class BookingService {
  async createBooking(booking: Booking) {
    // Create booking
    const created = await db.bookings.create(booking);

    // Invalidate related caches
    await Promise.all([
      cache.invalidate(CacheKeys.userBookings(booking.userId)),
      cache.invalidate(CacheKeys.barberAvailability(booking.barberId, booking.scheduledDate)),
      cache.invalidate(CacheKeys.shopQueue(booking.shopId))
    ]);

    return created;
  }
}
```

### Load Balancing

#### Application Load Balancer
```yaml
# AWS ALB / GCP Load Balancer
apiVersion: v1
kind: Service
metadata:
  name: cutclub-api-lb
spec:
  type: LoadBalancer
  selector:
    app: cutclub-api
  ports:
    - protocol: TCP
      port: 443
      targetPort: 8080
  sessionAffinity: ClientIP  # Sticky sessions for WebSocket
```

#### Health Checks
```typescript
// Express health check endpoint
app.get('/health', async (req, res) => {
  const checks = await Promise.all([
    checkDatabase(),
    checkRedis(),
    checkStorage(),
  ]);

  const isHealthy = checks.every(check => check.healthy);

  res.status(isHealthy ? 200 : 503).json({
    status: isHealthy ? 'healthy' : 'unhealthy',
    checks,
    timestamp: new Date().toISOString()
  });
});

async function checkDatabase() {
  try {
    await db.query('SELECT 1');
    return { name: 'database', healthy: true };
  } catch (error) {
    return { name: 'database', healthy: false, error: error.message };
  }
}
```

### Horizontal Scaling

#### Stateless Services
```typescript
// Design services to be stateless
class BookingService {
  // Bad: Stores state in memory
  // private bookingCache = new Map();

  // Good: Use external state store
  constructor(
    private cache: CacheService,
    private db: Database
  ) {}

  async getBooking(id: string) {
    // State in Redis, not in-memory
    return this.cache.get(`booking:${id}`) ||
           this.db.bookings.findById(id);
  }
}
```

#### Auto-Scaling Configuration
```yaml
# Kubernetes HPA
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: cutclub-api-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: cutclub-api
  minReplicas: 3
  maxReplicas: 20
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70
    - type: Resource
      resource:
        name: memory
        target:
          type: Utilization
          averageUtilization: 80
```

## Real-Time Features Performance

### WebSocket Optimization

#### Connection Management
```typescript
import { Server } from 'socket.io';

const io = new Server({
  cors: { origin: '*' },
  transports: ['websocket', 'polling'],
  pingTimeout: 60000,
  pingInterval: 25000,
  maxHttpBufferSize: 1e6, // 1MB
  perMessageDeflate: {
    threshold: 1024 // Compress messages > 1KB
  }
});

// Connection pooling by shop/feature
io.of('/queue').on('connection', (socket) => {
  socket.on('subscribe', ({ shopId }) => {
    socket.join(`shop:${shopId}`);
  });

  socket.on('unsubscribe', ({ shopId }) => {
    socket.leave(`shop:${shopId}`);
  });
});

// Emit only to relevant clients
function broadcastQueueUpdate(shopId: string, data: QueueUpdate) {
  io.of('/queue').to(`shop:${shopId}`).emit('queue:update', data);
}
```

#### Message Batching
```typescript
class MessageBatcher {
  private pending: Map<string, any[]> = new Map();
  private timers: Map<string, NodeJS.Timeout> = new Map();

  queue(room: string, event: string, data: any) {
    if (!this.pending.has(room)) {
      this.pending.set(room, []);
    }

    this.pending.get(room)!.push({ event, data });

    // Batch messages for 50ms
    if (!this.timers.has(room)) {
      this.timers.set(room, setTimeout(() => {
        this.flush(room);
      }, 50));
    }
  }

  private flush(room: string) {
    const messages = this.pending.get(room);
    if (messages && messages.length > 0) {
      io.to(room).emit('batch', messages);
      this.pending.delete(room);
    }
    this.timers.delete(room);
  }
}
```

### Queue Status Updates

#### Efficient Broadcasting
```typescript
// Problem: Broadcasting to all clients is expensive
// Solution: Rate limiting + throttling

class QueueManager {
  private lastBroadcast = new Map<string, number>();
  private readonly BROADCAST_INTERVAL = 1000; // 1 second

  async updateQueue(shopId: string, update: QueueUpdate) {
    // Update database
    await db.shops.update(shopId, { queueLength: update.queueLength });

    // Throttle broadcasts
    const lastTime = this.lastBroadcast.get(shopId) || 0;
    const now = Date.now();

    if (now - lastTime < this.BROADCAST_INTERVAL) {
      // Queue update for later
      return;
    }

    this.lastBroadcast.set(shopId, now);

    // Broadcast to connected clients only
    io.of('/queue')
      .to(`shop:${shopId}`)
      .emit('queue:update', update);
  }
}
```

## CDN & Asset Optimization

### Image Optimization

#### Responsive Images
```typescript
// Cloud Function for image resizing
import sharp from 'sharp';

export const resizeImage = functions.storage.object().onFinalize(async (object) => {
  const filePath = object.name;
  const contentType = object.contentType;

  if (!contentType?.startsWith('image/')) return;

  const sizes = [
    { suffix: 'thumb', width: 150, height: 150 },
    { suffix: 'small', width: 400, height: 400 },
    { suffix: 'medium', width: 800, height: 800 },
    { suffix: 'large', width: 1200, height: 1200 }
  ];

  await Promise.all(sizes.map(async (size) => {
    const resized = await sharp(filePath)
      .resize(size.width, size.height, { fit: 'cover' })
      .webp({ quality: 85 })
      .toBuffer();

    await bucket.file(`${filePath}_${size.suffix}.webp`).save(resized);
  }));
});
```

#### iOS Progressive Loading
```swift
func loadProgressiveImage(url: URL) {
    // Load thumbnail first (fast)
    let thumbUrl = url.appendingPathComponent("_thumb.webp")
    imageView.kf.setImage(with: thumbUrl)

    // Load full resolution (slower)
    let fullUrl = url.appendingPathComponent("_large.webp")
    imageView.kf.setImage(
        with: fullUrl,
        options: [.transition(.fade(0.3))]
    )
}
```

### Video Optimization

#### Adaptive Bitrate Streaming
```typescript
// FFmpeg transcoding to HLS
import ffmpeg from 'fluent-ffmpeg';

async function transcodeToHLS(inputPath: string, outputPath: string) {
  return new Promise((resolve, reject) => {
    ffmpeg(inputPath)
      .outputOptions([
        '-c:v libx264',
        '-c:a aac',
        '-hls_time 4',
        '-hls_playlist_type vod',
        '-hls_segment_filename', `${outputPath}/segment_%03d.ts`,
        '-master_pl_name', 'master.m3u8',
        // Multiple bitrates
        '-b:v:0 800k -maxrate:v:0 856k -bufsize:v:0 1200k',
        '-b:v:1 1400k -maxrate:v:1 1498k -bufsize:v:1 2100k',
        '-b:v:2 2800k -maxrate:v:2 2996k -bufsize:v:2 4200k'
      ])
      .output(`${outputPath}/master.m3u8`)
      .on('end', resolve)
      .on('error', reject)
      .run();
  });
}
```

## Monitoring & Observability

### Performance Metrics

#### Custom Metrics
```swift
import Firebase

class PerformanceMonitor {
    static func trackOperation(
        _ name: String,
        operation: () async throws -> Void
    ) async rethrows {
        let trace = Performance.startTrace(name: name)

        let start = Date()

        do {
            try await operation()
            trace?.setValue(1, forMetric: "success")
        } catch {
            trace?.setValue(0, forMetric: "success")
            throw error
        }

        let duration = Date().timeIntervalSince(start)
        trace?.setValue(Int64(duration * 1000), forMetric: "duration_ms")
        trace?.stop()
    }
}

// Usage
await PerformanceMonitor.trackOperation("create_booking") {
    try await bookingService.createBooking(booking)
}
```

#### Backend Metrics
```typescript
import { Histogram, Counter } from 'prom-client';

const httpDuration = new Histogram({
  name: 'http_request_duration_ms',
  help: 'Duration of HTTP requests in ms',
  labelNames: ['method', 'route', 'status_code'],
  buckets: [10, 50, 100, 200, 500, 1000, 2000]
});

const httpRequests = new Counter({
  name: 'http_requests_total',
  help: 'Total number of HTTP requests',
  labelNames: ['method', 'route', 'status_code']
});

// Middleware
app.use((req, res, next) => {
  const start = Date.now();

  res.on('finish', () => {
    const duration = Date.now() - start;

    httpDuration.observe(
      { method: req.method, route: req.route?.path, status_code: res.statusCode },
      duration
    );

    httpRequests.inc({
      method: req.method,
      route: req.route?.path,
      status_code: res.statusCode
    });
  });

  next();
});
```

### Alerting Thresholds

| Metric | Warning | Critical | Action |
|--------|---------|----------|--------|
| API Response Time (p95) | >200ms | >500ms | Scale up |
| Error Rate | >1% | >5% | Investigate |
| CPU Usage | >80% | >95% | Auto-scale |
| Memory Usage | >85% | >95% | Auto-scale |
| Database Connections | >80% of pool | >95% of pool | Increase pool |
| Queue Length | >10 | >20 | Add workers |
| WebSocket Connections | >5K | >10K | Add servers |

## Scalability Milestones

| Users | Infrastructure | Est. Monthly Cost | Notes |
|-------|----------------|-------------------|-------|
| 0-10K | Firebase + 1 CDN | $500-800 | MVP phase |
| 10K-30K | Firebase + CDN + Redis | $1,500-2,500 | Add caching |
| 30K-50K | Hybrid (Firebase + Custom) | $2,500-4,000 | Begin migration |
| 50K-100K | Custom Backend (3 nodes) | $3,000-5,000 | Full custom |
| 100K-500K | Custom Backend (10 nodes) | $8,000-15,000 | Multi-region |
| 500K+ | Enterprise (20+ nodes) | $20,000+ | Dedicated infrastructure |

## Next: Analytics Strategy
See `/docs/architecture/07-analytics.md` for complete analytics and monitoring plan.
