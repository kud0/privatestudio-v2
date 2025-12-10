# The Cut Club - Real-Time Features Architecture

## Overview

Real-time features are critical for The Cut Club's user experience:
1. **Queue Status**: Live wait time updates
2. **Booking Status**: Real-time appointment tracking
3. **Social Feed**: Live post updates and interactions
4. **DJ Mode**: Collaborative playlist voting
5. **Challenge Leaderboards**: Live ranking updates
6. **Notifications**: Instant push notifications

## Technology Choices

### Firebase Real-Time (Recommended for MVP)

**Firestore Real-Time Listeners**:
```swift
import FirebaseFirestore

class QueueMonitor {
    private var listener: ListenerRegistration?

    func observeQueue(shopId: String, callback: @escaping (QueueStatus) -> Void) {
        let queueRef = Firestore.firestore()
            .collection("shops")
            .document(shopId)

        // Real-time listener
        listener = queueRef.addSnapshotListener { snapshot, error in
            guard let data = snapshot?.data(),
                  let queue = try? QueueStatus(from: data) else {
                return
            }

            callback(queue)
        }
    }

    func stopObserving() {
        listener?.remove()
    }
}
```

**Benefits**:
- Automatic reconnection
- Offline support
- Simple implementation
- Security rules integration
- <50ms latency

**Limitations**:
- Costs $0.06 per 100K reads
- Limited to 1 million concurrent connections per database
- Can be expensive at scale

### WebSocket (Custom Backend Alternative)

**Socket.io Implementation**:
```typescript
import { Server } from 'socket.io';

const io = new Server(server, {
  cors: { origin: '*' },
  transports: ['websocket', 'polling'],
  pingTimeout: 60000,
  pingInterval: 25000
});

// Namespace for queue updates
const queueNamespace = io.of('/queue');

queueNamespace.on('connection', (socket) => {
  console.log('Client connected:', socket.id);

  // Authenticate
  socket.on('authenticate', async (token) => {
    try {
      const user = await verifyToken(token);
      socket.data.user = user;
      socket.emit('authenticated');
    } catch (error) {
      socket.disconnect();
    }
  });

  // Subscribe to shop queue
  socket.on('subscribe', ({ shopId }) => {
    if (!socket.data.user) return;

    socket.join(`shop:${shopId}`);
    socket.emit('subscribed', { shopId });
  });

  // Unsubscribe
  socket.on('unsubscribe', ({ shopId }) => {
    socket.leave(`shop:${shopId}`);
  });

  socket.on('disconnect', () => {
    console.log('Client disconnected:', socket.id);
  });
});

// Emit queue update to all subscribed clients
export function broadcastQueueUpdate(shopId: string, update: QueueStatus) {
  queueNamespace.to(`shop:${shopId}`).emit('queue:update', update);
}
```

**iOS WebSocket Client**:
```swift
import Starscream

class WebSocketManager: WebSocketDelegate {
    private var socket: WebSocket?
    private let url = URL(string: "wss://api.thecutclub.com/queue")!

    func connect(token: String) {
        var request = URLRequest(url: url)
        request.timeoutInterval = 10

        socket = WebSocket(request: request)
        socket?.delegate = self
        socket?.connect()
    }

    func subscribe(to shopId: String) {
        let message = [
            "event": "subscribe",
            "data": ["shopId": shopId]
        ]

        if let json = try? JSONSerialization.data(withJSONObject: message) {
            socket?.write(data: json)
        }
    }

    // Delegate methods
    func didReceive(event: WebSocketEvent, client: WebSocket) {
        switch event {
        case .connected:
            print("WebSocket connected")
            authenticate()

        case .text(let text):
            handleMessage(text)

        case .disconnected(let reason, let code):
            print("WebSocket disconnected: \(reason) (\(code))")
            handleReconnection()

        default:
            break
        }
    }

    private func handleMessage(_ text: String) {
        guard let data = text.data(using: .utf8),
              let message = try? JSONDecoder().decode(WebSocketMessage.self, from: data) else {
            return
        }

        switch message.event {
        case "queue:update":
            handleQueueUpdate(message.data)
        case "booking:status_change":
            handleBookingStatusChange(message.data)
        default:
            break
        }
    }

    private func handleReconnection() {
        // Exponential backoff
        var delay = 1.0
        let maxDelay = 30.0

        Timer.scheduledTimer(withTimeInterval: delay, repeats: false) { [weak self] _ in
            self?.socket?.connect()
            delay = min(delay * 2, maxDelay)
        }
    }
}
```

## Feature-Specific Implementations

### 1. Queue Status Updates

#### Data Model
```typescript
interface QueueStatus {
  shopId: string;
  queueLength: number;
  avgWaitTime: number;        // Minutes
  currentlyServing: number;    // Active barbers
  estimatedWaitTimes: {
    standard: number;
    blackCard: number;
  };
  updatedAt: Date;
}
```

#### Backend Update Logic
```typescript
class QueueService {
  async updateQueueStatus(shopId: string) {
    // Calculate current queue metrics
    const activeBookings = await db.bookings.find({
      shopId,
      status: { $in: ['confirmed', 'in-progress'] },
      scheduledDate: today
    });

    const queueLength = activeBookings.filter(b => b.status === 'confirmed').length;
    const activeBarbers = await getActiveBarbers(shopId);

    const avgServiceTime = await getAverageServiceTime(shopId);
    const avgWaitTime = calculateWaitTime(queueLength, activeBarbers.length, avgServiceTime);

    const status: QueueStatus = {
      shopId,
      queueLength,
      avgWaitTime,
      currentlyServing: activeBarbers.length,
      estimatedWaitTimes: {
        standard: avgWaitTime,
        blackCard: Math.max(avgWaitTime * 0.4, 5) // Black Card skip-the-line
      },
      updatedAt: new Date()
    };

    // Update Firestore (triggers real-time listeners)
    await db.collection('queue_status').doc(shopId).set(status);

    // OR broadcast via WebSocket
    broadcastQueueUpdate(shopId, status);

    // Cache for API requests
    await redis.setex(`queue:${shopId}`, 60, JSON.stringify(status));
  }

  private calculateWaitTime(
    queueLength: number,
    activeBarbers: number,
    avgServiceTime: number
  ): number {
    if (activeBarbers === 0) return 0;

    // Simple formula: (queue length / active barbers) * avg service time
    return Math.ceil((queueLength / activeBarbers) * avgServiceTime);
  }
}
```

#### iOS UI
```swift
struct QueueStatusView: View {
    @StateObject private var viewModel = QueueViewModel()
    let shopId: String

    var body: some View {
        VStack(spacing: 16) {
            // Current wait time
            VStack {
                Text(viewModel.waitTime)
                    .font(.system(size: 48, weight: .bold))
                    .foregroundColor(.gold)

                Text("Estimated Wait")
                    .font(.subheadline)
                    .foregroundColor(.secondary)
            }
            .padding()
            .background(Color.black.opacity(0.3))
            .cornerRadius(16)

            // Queue details
            HStack(spacing: 32) {
                QueueStat(
                    icon: "person.2.fill",
                    value: "\(viewModel.queueLength)",
                    label: "In Queue"
                )

                QueueStat(
                    icon: "scissors",
                    value: "\(viewModel.activeBarbers)",
                    label: "Active"
                )
            }

            // Real-time indicator
            HStack {
                Circle()
                    .fill(Color.green)
                    .frame(width: 8, height: 8)

                Text("Live Updates")
                    .font(.caption)
                    .foregroundColor(.secondary)
            }
        }
        .onAppear {
            viewModel.startObserving(shopId: shopId)
        }
        .onDisappear {
            viewModel.stopObserving()
        }
    }
}

class QueueViewModel: ObservableObject {
    @Published var queueLength: Int = 0
    @Published var waitTime: String = "0 min"
    @Published var activeBarbers: Int = 0

    private var listener: ListenerRegistration?

    func startObserving(shopId: String) {
        let ref = Firestore.firestore()
            .collection("queue_status")
            .document(shopId)

        listener = ref.addSnapshotListener { [weak self] snapshot, error in
            guard let data = snapshot?.data() else { return }

            self?.queueLength = data["queueLength"] as? Int ?? 0
            self?.activeBarbers = data["currentlyServing"] as? Int ?? 0

            let minutes = data["avgWaitTime"] as? Int ?? 0
            self?.waitTime = "\(minutes) min"
        }
    }

    func stopObserving() {
        listener?.remove()
    }
}
```

### 2. Booking Status Tracking

#### Status Flow
```
pending → confirmed → in-progress → completed
         ↓
      cancelled
         ↓
      no-show
```

#### Backend Updates
```typescript
class BookingStatusService {
  async updateBookingStatus(bookingId: string, status: BookingStatus) {
    const booking = await db.bookings.findById(bookingId);

    // Update status
    await db.bookings.update(bookingId, {
      status,
      updatedAt: new Date()
    });

    // Real-time notification to user
    await this.notifyStatusChange(booking.userId, bookingId, status);

    // Update queue if needed
    if (status === 'completed' || status === 'cancelled') {
      await queueService.updateQueueStatus(booking.shopId);
    }
  }

  private async notifyStatusChange(
    userId: string,
    bookingId: string,
    status: BookingStatus
  ) {
    // Firebase: Write to user's notifications subcollection
    await db.collection('users')
      .doc(userId)
      .collection('notifications')
      .add({
        type: 'booking_status_change',
        bookingId,
        status,
        timestamp: new Date()
      });

    // Push notification
    await sendPushNotification(userId, {
      title: getStatusTitle(status),
      body: getStatusMessage(status),
      data: { bookingId }
    });

    // WebSocket: Emit to user's connection
    io.of('/bookings')
      .to(`user:${userId}`)
      .emit('booking:status_change', { bookingId, status });
  }
}
```

#### iOS Observing
```swift
class BookingTracker: ObservableObject {
    @Published var status: BookingStatus = .pending
    @Published var estimatedCompletion: Date?

    private var listener: ListenerRegistration?

    func trackBooking(_ bookingId: String) {
        let ref = Firestore.firestore()
            .collection("bookings")
            .document(bookingId)

        listener = ref.addSnapshotListener { [weak self] snapshot, error in
            guard let data = snapshot?.data() else { return }

            if let statusStr = data["status"] as? String,
               let newStatus = BookingStatus(rawValue: statusStr) {
                self?.status = newStatus
                self?.handleStatusChange(newStatus)
            }

            if let timestamp = data["estimatedCompletion"] as? Timestamp {
                self?.estimatedCompletion = timestamp.dateValue()
            }
        }
    }

    private func handleStatusChange(_ status: BookingStatus) {
        // Update UI
        withAnimation {
            self.status = status
        }

        // Haptic feedback
        let haptic = UINotificationFeedbackGenerator()
        haptic.notificationOccurred(.success)

        // Local notification if app in background
        if UIApplication.shared.applicationState != .active {
            sendLocalNotification(status)
        }
    }
}
```

### 3. Social Feed Live Updates

#### Real-Time Post Updates
```typescript
// When barber creates new post
async function createPost(post: Post) {
  // Save to database
  const created = await db.posts.create(post);

  // Notify followers in real-time
  const followers = await getFollowers(post.barberId);

  followers.forEach(followerId => {
    // Firebase: Add to follower's feed
    db.collection('users')
      .doc(followerId)
      .collection('feed_updates')
      .add({
        type: 'new_post',
        postId: created.id,
        barberId: post.barberId,
        timestamp: new Date()
      });

    // WebSocket: Emit to active followers
    io.of('/social')
      .to(`user:${followerId}`)
      .emit('feed:new_post', created);
  });

  return created;
}
```

#### iOS Live Feed
```swift
struct LiveFeedView: View {
    @StateObject private var viewModel = FeedViewModel()

    var body: some View {
        ZStack(alignment: .top) {
            List(viewModel.posts) { post in
                PostCell(post: post)
            }

            // New posts banner
            if viewModel.hasNewPosts {
                Button(action: {
                    withAnimation {
                        viewModel.loadNewPosts()
                    }
                }) {
                    HStack {
                        Image(systemName: "arrow.up.circle.fill")
                        Text("\(viewModel.newPostCount) new posts")
                    }
                    .padding()
                    .background(Color.gold)
                    .foregroundColor(.black)
                    .cornerRadius(20)
                }
                .transition(.move(edge: .top))
            }
        }
    }
}

class FeedViewModel: ObservableObject {
    @Published var posts: [Post] = []
    @Published var hasNewPosts = false
    @Published var newPostCount = 0

    private var newPosts: [Post] = []
    private var listener: ListenerRegistration?

    func startListening() {
        // Listen for new posts
        listener = Firestore.firestore()
            .collection("users")
            .document(currentUserId)
            .collection("feed_updates")
            .order(by: "timestamp", descending: true)
            .limit(to: 1)
            .addSnapshotListener { [weak self] snapshot, error in
                guard let docs = snapshot?.documents else { return }

                for doc in docs {
                    let update = try? doc.data(as: FeedUpdate.self)
                    if let postId = update?.postId {
                        self?.fetchNewPost(postId)
                    }
                }
            }
    }

    private func fetchNewPost(_ postId: String) {
        Task {
            let post = try await API.fetchPost(postId)

            await MainActor.run {
                newPosts.insert(post, at: 0)
                newPostCount = newPosts.count
                hasNewPosts = true

                // Auto-load after 5 seconds if user doesn't tap
                DispatchQueue.main.asyncAfter(deadline: .now() + 5) { [weak self] in
                    if self?.hasNewPosts == true {
                        self?.loadNewPosts()
                    }
                }
            }
        }
    }

    func loadNewPosts() {
        posts.insert(contentsOf: newPosts, at: 0)
        newPosts.removeAll()
        hasNewPosts = false
        newPostCount = 0
    }
}
```

### 4. DJ Mode Playlist Voting

#### Data Model
```typescript
interface DJSession {
  shopId: string;
  isActive: boolean;
  currentTrack: SpotifyTrack;
  queue: DJQueueItem[];
  startedAt: Date;
}

interface DJQueueItem {
  trackId: string;
  trackName: string;
  artistName: string;
  albumArt: string;
  addedBy: string;
  upvotes: string[];        // User IDs
  downvotes: string[];      // User IDs
  score: number;            // upvotes.length - downvotes.length
  addedAt: Date;
}
```

#### Real-Time Voting
```typescript
// WebSocket handler for DJ voting
djNamespace.on('connection', (socket) => {
  socket.on('vote', async ({ shopId, trackId, vote }) => {
    const userId = socket.data.user.id;

    // Validate user is at the shop
    const isPresent = await checkUserPresence(userId, shopId);
    if (!isPresent) {
      socket.emit('error', { message: 'Must be at shop to vote' });
      return;
    }

    // Update vote
    await db.collection('dj_sessions')
      .doc(shopId)
      .collection('queue')
      .doc(trackId)
      .update({
        [`${vote}votes`]: admin.firestore.FieldValue.arrayUnion(userId),
        // Remove from opposite vote array
        [`${vote === 'up' ? 'down' : 'up'}votes`]: admin.firestore.FieldValue.arrayRemove(userId)
      });

    // Recalculate score and broadcast
    const updatedItem = await getQueueItem(shopId, trackId);

    djNamespace.to(`shop:${shopId}`).emit('queue:update', {
      trackId,
      upvotes: updatedItem.upvotes.length,
      downvotes: updatedItem.downvotes.length,
      score: updatedItem.score,
      position: updatedItem.position
    });
  });

  socket.on('add_track', async ({ shopId, trackId }) => {
    // Add track to queue
    const track = await spotify.getTrack(trackId);

    await db.collection('dj_sessions')
      .doc(shopId)
      .collection('queue')
      .add({
        trackId,
        trackName: track.name,
        artistName: track.artists[0].name,
        albumArt: track.album.images[0].url,
        addedBy: socket.data.user.id,
        upvotes: [socket.data.user.id],
        downvotes: [],
        score: 1,
        addedAt: new Date()
      });

    // Broadcast new track
    djNamespace.to(`shop:${shopId}`).emit('track:added', track);
  });
});
```

#### iOS DJ Mode UI
```swift
struct DJModeView: View {
    @StateObject private var viewModel = DJModeViewModel()
    let shopId: String

    var body: some View {
        VStack {
            // Now Playing
            NowPlayingCard(track: viewModel.currentTrack)

            // Queue
            List(viewModel.queue) { item in
                DJQueueRow(
                    item: item,
                    onVote: { vote in
                        viewModel.vote(trackId: item.trackId, vote: vote)
                    }
                )
            }

            // Add Track Button
            Button("Add Track") {
                viewModel.showTrackPicker = true
            }
        }
        .onAppear {
            viewModel.connect(shopId: shopId)
        }
    }
}

struct DJQueueRow: View {
    let item: DJQueueItem
    let onVote: (Vote) -> Void

    var body: some View {
        HStack {
            // Album Art
            AsyncImage(url: URL(string: item.albumArt)) { image in
                image.resizable()
            } placeholder: {
                Color.gray
            }
            .frame(width: 50, height: 50)
            .cornerRadius(8)

            // Track Info
            VStack(alignment: .leading) {
                Text(item.trackName)
                    .font(.headline)
                Text(item.artistName)
                    .font(.subheadline)
                    .foregroundColor(.secondary)
            }

            Spacer()

            // Vote Controls
            HStack {
                Button(action: { onVote(.down) }) {
                    Image(systemName: "arrow.down.circle.fill")
                        .foregroundColor(item.userVote == .down ? .red : .gray)
                }

                Text("\(item.score)")
                    .font(.headline)
                    .frame(width: 40)

                Button(action: { onVote(.up) }) {
                    Image(systemName: "arrow.up.circle.fill")
                        .foregroundColor(item.userVote == .up ? .green : .gray)
                }
            }
        }
    }
}

class DJModeViewModel: ObservableObject {
    @Published var currentTrack: SpotifyTrack?
    @Published var queue: [DJQueueItem] = []

    private var socket: WebSocket?

    func connect(shopId: String) {
        // Connect to WebSocket
        socket = WebSocketManager.shared.connect(to: "/dj")
        socket?.emit("subscribe", ["shopId": shopId])

        // Listen for updates
        socket?.on("queue:update") { [weak self] data in
            self?.handleQueueUpdate(data)
        }

        socket?.on("track:playing") { [weak self] data in
            self?.handleTrackChange(data)
        }
    }

    func vote(trackId: String, vote: Vote) {
        socket?.emit("vote", [
            "trackId": trackId,
            "vote": vote.rawValue
        ])

        // Optimistic update
        if let index = queue.firstIndex(where: { $0.trackId == trackId }) {
            queue[index].updateVote(vote)
        }
    }
}
```

### 5. Challenge Leaderboards

#### Real-Time Ranking Updates
```typescript
// Update leaderboard when user earns points
async function updateChallengeProgress(
  challengeId: string,
  userId: string,
  points: number
) {
  // Update user's score
  const leaderboardRef = db
    .collection('challenges')
    .doc(challengeId)
    .collection('leaderboard')
    .doc(userId);

  await leaderboardRef.set({
    userId,
    score: admin.firestore.FieldValue.increment(points),
    lastUpdated: new Date()
  }, { merge: true });

  // Recalculate rankings (Cloud Function)
  await recalculateRankings(challengeId);

  // Notify affected users (top 100)
  const topUsers = await getTopUsers(challengeId, 100);

  topUsers.forEach(user => {
    io.of('/challenges')
      .to(`user:${user.userId}`)
      .emit('leaderboard:update', {
        challengeId,
        rank: user.rank,
        score: user.score
      });
  });
}

// Efficient ranking calculation
async function recalculateRankings(challengeId: string) {
  // Use Redis sorted set for fast rankings
  const entries = await db
    .collection('challenges')
    .doc(challengeId)
    .collection('leaderboard')
    .get();

  const multi = redis.multi();

  entries.docs.forEach(doc => {
    const { userId, score } = doc.data();
    multi.zadd(`leaderboard:${challengeId}`, score, userId);
  });

  await multi.exec();
}

// Get user's rank efficiently
async function getUserRank(challengeId: string, userId: string): Promise<number> {
  const rank = await redis.zrevrank(`leaderboard:${challengeId}`, userId);
  return rank !== null ? rank + 1 : -1;
}
```

#### iOS Live Leaderboard
```swift
struct LeaderboardView: View {
    @StateObject private var viewModel = LeaderboardViewModel()
    let challengeId: String

    var body: some View {
        List {
            // Current user's rank
            if let userRank = viewModel.currentUserRank {
                Section {
                    LeaderboardRow(
                        rank: userRank.rank,
                        user: userRank.user,
                        score: userRank.score,
                        isCurrentUser: true
                    )
                }
                .listRowBackground(Color.gold.opacity(0.2))
            }

            // Top users
            Section("Top \(viewModel.topUsers.count)") {
                ForEach(viewModel.topUsers) { entry in
                    LeaderboardRow(
                        rank: entry.rank,
                        user: entry.user,
                        score: entry.score,
                        isCurrentUser: false
                    )
                }
            }
        }
        .onAppear {
            viewModel.startObserving(challengeId: challengeId)
        }
    }
}

class LeaderboardViewModel: ObservableObject {
    @Published var topUsers: [LeaderboardEntry] = []
    @Published var currentUserRank: LeaderboardEntry?

    private var listener: ListenerRegistration?

    func startObserving(challengeId: String) {
        // Observe top 100
        listener = Firestore.firestore()
            .collection("challenges")
            .document(challengeId)
            .collection("leaderboard")
            .order(by: "score", descending: true)
            .limit(to: 100)
            .addSnapshotListener { [weak self] snapshot, error in
                guard let docs = snapshot?.documents else { return }

                var entries: [LeaderboardEntry] = []

                for (index, doc) in docs.enumerated() {
                    let data = doc.data()
                    let entry = LeaderboardEntry(
                        rank: index + 1,
                        userId: doc.documentID,
                        score: data["score"] as? Int ?? 0
                    )
                    entries.append(entry)

                    // Track current user
                    if entry.userId == Auth.auth().currentUser?.uid {
                        self?.currentUserRank = entry
                    }
                }

                self?.topUsers = entries
            }
    }
}
```

## Connection Management

### Handling Network Changes

```swift
import Network

class NetworkMonitor: ObservableObject {
    private let monitor = NWPathMonitor()
    private let queue = DispatchQueue(label: "NetworkMonitor")

    @Published var isConnected = true
    @Published var connectionType: NWInterface.InterfaceType?

    init() {
        monitor.pathUpdateHandler = { [weak self] path in
            DispatchQueue.main.async {
                self?.isConnected = path.status == .satisfied
                self?.connectionType = path.availableInterfaces.first?.type
            }

            // Handle reconnection
            if path.status == .satisfied {
                self?.handleReconnection()
            }
        }

        monitor.start(queue: queue)
    }

    private func handleReconnection() {
        // Reconnect WebSockets
        WebSocketManager.shared.reconnect()

        // Re-subscribe to Firestore listeners
        FirestoreManager.shared.resubscribe()

        // Sync offline changes
        OfflineManager.shared.syncPendingChanges()
    }
}
```

### Automatic Retry Logic

```swift
class ReliableWebSocket {
    private var socket: WebSocket?
    private var retryCount = 0
    private let maxRetries = 10

    func connect() {
        socket?.connect()
    }

    func handleDisconnection() {
        guard retryCount < maxRetries else {
            showConnectionError()
            return
        }

        // Exponential backoff: 1s, 2s, 4s, 8s, ...
        let delay = min(pow(2.0, Double(retryCount)), 30.0)
        retryCount += 1

        DispatchQueue.main.asyncAfter(deadline: .now() + delay) { [weak self] in
            self?.connect()
        }
    }

    func didConnect() {
        retryCount = 0 // Reset on successful connection
    }
}
```

## Next: Analytics & Monitoring
See `/docs/architecture/08-analytics-monitoring.md` for complete tracking strategy.
