# Section 4: Architecture & System Design

## 4.1 App Architecture — MVVM + Clean Architecture

### Layer Structure
```
app/
├── data/               # Data layer
│   ├── remote/         # Retrofit APIs, DTOs
│   ├── local/          # Room DAOs, entities
│   └── repository/     # Impl of domain repositories
│
├── domain/             # Business logic (pure Kotlin, no Android deps)
│   ├── model/          # Domain models
│   ├── repository/     # Interfaces
│   └── usecase/        # Single-purpose use cases
│
├── presentation/       # UI layer
│   ├── ui/
│   │   ├── screens/    # Composables or Fragments
│   │   └── components/ # Reusable UI components
│   └── viewmodel/      # ViewModels + UI state
│
├── di/                 # Dependency injection modules
└── utils/              # Extensions, constants, helpers
```

### Data Flow (unidirectional)
```
UI → ViewModel → UseCase → Repository → DataSource
                                             ↓
UI ← ViewModel ← UseCase ← Repository ← DataSource
```

### Key Rules
- **ViewModels** own UI state — never access Context
- **UseCases** contain business logic — one public function each
- **Repositories** abstract data source — UI never knows if data is local or remote
- **Data layer** handles mapping — DTOs never leak into domain
- **Domain layer** has zero Android imports

---

## 4.2 Backend Architecture

### Decision: Monolith vs Microservices

| Criterion | Monolith | Microservices |
|-----------|----------|---------------|
| Team size | 1–5 devs | 5+ devs |
| MVP stage | ✅ Recommended | ❌ Over-engineering |
| Scale stage | Outgrows quickly | ✅ Built for scale |
| Deployment complexity | Low | High |
| When to migrate | >10k DAU or team splits | — |

### Recommended Backend Stack (MVP → Scale Path)
```
MVP:       Firebase (Firestore + Auth + Storage + Functions)
           OR Supabase (PostgreSQL + Auth + Storage + Edge Functions)

Scale v1:  Node.js/NestJS monolith + PostgreSQL + Redis
           Deployed on Railway / Render / Fly.io

Scale v2:  Extract heavy services into microservices
           (notifications, media processing, analytics)
           API Gateway (Kong / AWS API GW) in front
```

---

## 4.3 API Design Standards

### REST Conventions
```
GET    /v1/users/:id          → Fetch user
POST   /v1/users              → Create user
PUT    /v1/users/:id          → Replace user
PATCH  /v1/users/:id          → Update fields
DELETE /v1/users/:id          → Delete user

Nested: GET /v1/users/:id/orders
Filters: GET /v1/products?category=books&sort=price&page=2&limit=20
```

### Response Envelope
```json
{
  "success": true,
  "data": { ... },
  "meta": {
    "page": 1,
    "total": 243,
    "per_page": 20
  },
  "error": null
}

Error response:
{
  "success": false,
  "data": null,
  "error": {
    "code": "AUTH_INVALID_TOKEN",
    "message": "Token expired",
    "details": {}
  }
}
```

### API Versioning
- Prefix all routes: `/v1/`, `/v2/`
- Never break existing endpoints — add new versions
- Sunset old versions with 6-month deprecation notice

### Android-Side API Checklist
- [ ] Retrofit + OkHttp configured
- [ ] Auth interceptor adds Bearer token to all requests
- [ ] Timeout values set (connect: 10s, read: 30s, write: 30s)
- [ ] Retry logic for network failures (exponential backoff)
- [ ] Response sealed class: `Success<T>`, `Error`, `Loading`
- [ ] No API calls in ViewModel — go through UseCase → Repository

---

## 4.4 Database Schema Design

### Schema Design Checklist
- [ ] Every table has `id` (UUID preferred), `created_at`, `updated_at`
- [ ] Soft deletes: `deleted_at` nullable (don't hard delete user data)
- [ ] Indexes on: foreign keys, frequently queried fields, sort columns
- [ ] No plain-text passwords — bcrypt hash only
- [ ] PII fields identified — encrypted at rest if required
- [ ] Schema migration strategy defined (Flyway / Liquibase / Room migrations)

### Room (Local DB) Conventions
```kotlin
// Entity
@Entity(tableName = "users")
data class UserEntity(
    @PrimaryKey val id: String,
    val name: String,
    val email: String,
    @ColumnInfo(name = "created_at") val createdAt: Long
)

// DAO
@Dao
interface UserDao {
    @Query("SELECT * FROM users WHERE id = :id")
    suspend fun getById(id: String): UserEntity?

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun upsert(user: UserEntity)
}
```

---

## 4.5 Scalability Considerations

### Mobile-Side Scalability
| Concern | Solution |
|---------|---------|
| Memory leaks | ViewModels + lifecycle-aware observers |
| Large lists | LazyColumn / RecyclerView with DiffUtil |
| Image loading | Coil (Kotlin) / Glide — disk + memory cache |
| Background work | WorkManager for deferrable tasks |
| Battery drain | Batch network calls, avoid wakelock abuse |

### Backend-Side Scalability
| Concern | Solution |
|---------|---------|
| Database N+1 | Eager loading / DataLoader pattern |
| High traffic | Horizontal scaling + load balancer |
| Expensive queries | Redis caching (TTL-based) |
| File uploads | Direct-to-S3 signed URLs — don't route through API |
| Push at scale | FCM topics / bulk send — not individual calls |

### Offline-First Architecture (if required)
```
Strategy: Local-first, sync-on-connect

1. All writes go to Room first (immediate UI feedback)
2. WorkManager queues sync job
3. On network available → sync queue → resolve conflicts
4. Conflict resolution: Last-write-wins OR server-wins (define per entity)
```
