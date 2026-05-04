# Section 5: Development Phase

## 5.1 Project Setup

### Android Project Checklist (Kotlin + Compose)
- [ ] `minSdk` set (API 26 recommended), `targetSdk` = latest stable
- [ ] Package name follows convention: `com.company.appname`
- [ ] Version code / name strategy defined (see Section 7)
- [ ] Gradle version catalog configured (`libs.versions.toml`)
- [ ] `.gitignore` includes: `local.properties`, `*.jks`, `google-services.json`
- [ ] `google-services.json` in `.gitignore` (use CI secrets instead)
- [ ] ProGuard/R8 rules configured for release build
- [ ] Signing config in `local.properties` (never hardcode in `build.gradle`)

### Essential Dependencies (Kotlin)
```toml
# libs.versions.toml
[versions]
kotlin = "1.9.x"
compose-bom = "2024.x.x"
hilt = "2.x"
retrofit = "2.9.0"
room = "2.6.x"
coroutines = "1.7.x"
coil = "2.x"

[libraries]
# Compose
compose-bom = { group = "androidx.compose", name = "compose-bom", version.ref = "compose-bom" }
compose-ui = { group = "androidx.compose.ui", name = "ui" }
compose-material3 = { group = "androidx.compose.material3", name = "material3" }

# Architecture
hilt-android = { group = "com.google.dagger", name = "hilt-android", version.ref = "hilt" }
lifecycle-viewmodel = { group = "androidx.lifecycle", name = "lifecycle-viewmodel-ktx" }

# Network
retrofit = { group = "com.squareup.retrofit2", name = "retrofit", version.ref = "retrofit" }
okhttp-logging = { group = "com.squareup.okhttp3", name = "logging-interceptor" }

# Database
room-runtime = { group = "androidx.room", name = "room-runtime", version.ref = "room" }
room-ktx = { group = "androidx.room", name = "room-ktx", version.ref = "room" }

# Image
coil = { group = "io.coil-kt", name = "coil-compose", version.ref = "coil" }
```

---

## 5.2 Folder Organization

```
app/src/main/
├── java/com/company/appname/
│   ├── MainActivity.kt
│   ├── App.kt                    # Application class
│   │
│   ├── data/
│   │   ├── remote/
│   │   │   ├── api/              # Retrofit interfaces
│   │   │   ├── dto/              # Request/response data classes
│   │   │   └── interceptor/      # Auth, logging interceptors
│   │   ├── local/
│   │   │   ├── dao/              # Room DAOs
│   │   │   ├── entity/           # Room entities
│   │   │   └── AppDatabase.kt
│   │   ├── repository/           # Repository implementations
│   │   └── mapper/               # DTO ↔ Domain model mappers
│   │
│   ├── domain/
│   │   ├── model/                # Clean domain models
│   │   ├── repository/           # Repository interfaces
│   │   └── usecase/              # One class per use case
│   │
│   ├── presentation/
│   │   ├── navigation/           # NavGraph, Routes
│   │   ├── theme/                # Color, Type, Theme.kt
│   │   ├── components/           # Shared composables
│   │   └── feature/              # One folder per feature
│   │       └── home/
│   │           ├── HomeScreen.kt
│   │           ├── HomeViewModel.kt
│   │           └── HomeUiState.kt
│   │
│   ├── di/                       # Hilt modules
│   └── util/
│       ├── extensions/
│       ├── Constants.kt
│       └── Result.kt             # Sealed class for API results
│
└── res/
    ├── drawable/
    ├── values/
    │   ├── strings.xml
    │   ├── colors.xml            # Reference colors only
    │   └── themes.xml
    └── font/
```

---

## 5.3 State Management

### UI State Pattern
```kotlin
// UiState sealed class per screen
sealed class HomeUiState {
    object Loading : HomeUiState()
    data class Success(val items: List<Item>) : HomeUiState()
    data class Error(val message: String) : HomeUiState()
    object Empty : HomeUiState()
}

// ViewModel
@HiltViewModel
class HomeViewModel @Inject constructor(
    private val getItemsUseCase: GetItemsUseCase
) : ViewModel() {

    private val _uiState = MutableStateFlow<HomeUiState>(HomeUiState.Loading)
    val uiState: StateFlow<HomeUiState> = _uiState.asStateFlow()

    init { loadItems() }

    fun loadItems() {
        viewModelScope.launch {
            _uiState.value = HomeUiState.Loading
            getItemsUseCase()
                .onSuccess { _uiState.value = HomeUiState.Success(it) }
                .onFailure { _uiState.value = HomeUiState.Error(it.message ?: "Unknown error") }
        }
    }
}
```

---

## 5.4 API Integration

### Result Wrapper
```kotlin
sealed class Result<out T> {
    data class Success<T>(val data: T) : Result<T>()
    data class Error(val exception: Throwable, val code: Int? = null) : Result<Nothing>()
}

// Repository usage
override suspend fun getUser(id: String): Result<User> {
    return try {
        val response = api.getUser(id)
        if (response.isSuccessful) {
            Result.Success(response.body()!!.toDomain())
        } else {
            Result.Error(Exception("API error ${response.code()}"), response.code())
        }
    } catch (e: Exception) {
        Result.Error(e)
    }
}
```

### Network interceptor
```kotlin
class AuthInterceptor @Inject constructor(
    private val tokenProvider: TokenProvider
) : Interceptor {
    override fun intercept(chain: Interceptor.Chain): Response {
        val request = chain.request().newBuilder()
            .addHeader("Authorization", "Bearer ${tokenProvider.getToken()}")
            .addHeader("Content-Type", "application/json")
            .build()
        return chain.proceed(request)
    }
}
```

---

## 5.5 Authentication & Authorization

### Auth Checklist
- [ ] Token stored in `EncryptedSharedPreferences` (never plain SharedPrefs)
- [ ] Token refresh logic implemented (intercept 401 → refresh → retry)
- [ ] Logout clears all local data (Room + DataStore + memory)
- [ ] Biometric auth implemented for sensitive screens (BiometricPrompt API)
- [ ] Deep link auth callbacks handled (OAuth redirect URIs)
- [ ] Session timeout configured

### Firebase Auth Setup
```kotlin
// Sign in
FirebaseAuth.getInstance().signInWithEmailAndPassword(email, password)
    .addOnSuccessListener { result ->
        result.user?.getIdToken(false)?.addOnSuccessListener { tokenResult ->
            tokenProvider.saveToken(tokenResult.token ?: "")
        }
    }
    .addOnFailureListener { handleAuthError(it) }
```

---

## 5.6 Error Handling & Logging

### Error Categories
| Type | Strategy |
|------|---------|
| Network timeout | Retry with exponential backoff (max 3 attempts) |
| 401 Unauthorized | Token refresh → retry → force logout |
| 404 Not Found | Show empty state UI |
| 500 Server Error | Show "Something went wrong" + retry button |
| No internet | Show offline banner + queue for sync |
| Parse error | Log to Crashlytics + show generic error |

### Logging Strategy
```kotlin
// Timber setup (debug only logging)
if (BuildConfig.DEBUG) {
    Timber.plant(Timber.DebugTree())
} else {
    Timber.plant(CrashlyticsTree()) // Custom tree → Crashlytics
}

// Usage
Timber.d("Fetching user %s", userId)        // Debug
Timber.e(exception, "Failed to load feed")  // Error → Crashlytics in prod
```

### Crash Reporting Setup (Firebase Crashlytics)
- [ ] `google-services.json` configured
- [ ] Non-fatal exceptions logged: `FirebaseCrashlytics.getInstance().recordException(e)`
- [ ] Custom keys added for context: `setCustomKey("user_id", userId)`
- [ ] Crashlytics disabled for debug builds
