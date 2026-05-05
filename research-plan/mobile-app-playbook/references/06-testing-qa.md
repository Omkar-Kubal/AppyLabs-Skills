# Section 6: Testing & Quality Assurance

## 6.1 Testing Pyramid

```
        /\
       /UI\         ← Fewest tests (slow, expensive)
      /----\
     / Integ\       ← Moderate
    /--------\
   / Unit     \     ← Most tests (fast, cheap)
  /____________\

Target ratio: 70% Unit / 20% Integration / 10% UI
```

---

## 6.2 Unit Testing

### What to Unit Test
- UseCases (business logic)
- ViewModels (state transformations)
- Mappers (DTO → Domain)
- Utility functions
- Repository logic (with mocked data sources)

### Setup
```kotlin
// build.gradle
testImplementation("junit:junit:4.13.2")
testImplementation("io.mockk:mockk:1.13.x")
testImplementation("org.jetbrains.kotlinx:kotlinx-coroutines-test:1.7.x")
testImplementation("app.cash.turbine:turbine:1.x")  // StateFlow testing
```

### ViewModel Test Template
```kotlin
@OptIn(ExperimentalCoroutinesApi::class)
class HomeViewModelTest {

    @get:Rule
    val coroutineRule = MainDispatcherRule()

    private val getItemsUseCase: GetItemsUseCase = mockk()
    private lateinit var viewModel: HomeViewModel

    @Before
    fun setup() {
        viewModel = HomeViewModel(getItemsUseCase)
    }

    @Test
    fun `loading items emits success state`() = runTest {
        val items = listOf(Item(id = "1", name = "Test"))
        coEvery { getItemsUseCase() } returns Result.Success(items)

        viewModel.uiState.test {
            viewModel.loadItems()
            assertEquals(HomeUiState.Loading, awaitItem())
            assertEquals(HomeUiState.Success(items), awaitItem())
            cancelAndIgnoreRemainingEvents()
        }
    }
}
```

### Unit Test Checklist
- [ ] Each UseCase has ≥1 happy path + ≥1 failure path test
- [ ] Each ViewModel state transition covered
- [ ] All mapper functions tested with edge cases
- [ ] No Android framework imports in unit tests

---

## 6.3 Integration Testing

### What to Integration Test
- Repository (real Room DB + mocked API)
- API responses (MockWebServer)
- Navigation flows

### Room Integration Test
```kotlin
@RunWith(AndroidJUnit4::class)
class UserDaoTest {

    private lateinit var db: AppDatabase
    private lateinit var userDao: UserDao

    @Before
    fun setup() {
        db = Room.inMemoryDatabaseBuilder(
            ApplicationProvider.getApplicationContext(),
            AppDatabase::class.java
        ).build()
        userDao = db.userDao()
    }

    @After
    fun teardown() = db.close()

    @Test
    fun insertAndRetrieveUser() = runTest {
        val user = UserEntity(id = "1", name = "Alice", email = "a@b.com", createdAt = 0)
        userDao.upsert(user)
        assertEquals(user, userDao.getById("1"))
    }
}
```

---

## 6.4 UI Testing (Compose)

### Compose UI Test Template
```kotlin
@RunWith(AndroidJUnit4::class)
class HomeScreenTest {

    @get:Rule
    val composeRule = createComposeRule()

    @Test
    fun homeScreen_displaysItems_whenDataLoaded() {
        val items = listOf(Item("1", "Test Item"))
        composeRule.setContent {
            HomeScreen(uiState = HomeUiState.Success(items))
        }
        composeRule.onNodeWithText("Test Item").assertIsDisplayed()
    }

    @Test
    fun homeScreen_showsLoader_whenLoading() {
        composeRule.setContent {
            HomeScreen(uiState = HomeUiState.Loading)
        }
        composeRule.onNodeWithTag("loading_indicator").assertIsDisplayed()
    }
}
```

### UI Test Checklist
- [ ] All screens have loading state test
- [ ] All screens have empty state test
- [ ] All screens have error state test
- [ ] Primary CTA tapped → correct navigation verified
- [ ] Accessibility: contentDescription on all images/icons tested

---

## 6.5 Performance Testing

### Tools
| Tool | Purpose |
|------|---------|
| Android Profiler (CPU) | Detect janky frames, method traces |
| Android Profiler (Memory) | Leak detection, allocations |
| Android Profiler (Network) | Request timing, payload sizes |
| LeakCanary | Automatic memory leak detection in debug |
| Macrobenchmark | Startup time, scroll performance |
| Firebase Performance | Production performance monitoring |

### Performance Targets
| Metric | Target |
|--------|--------|
| App cold start | < 2 seconds |
| Screen render | 60fps (16ms per frame) |
| ANR (App Not Responding) | 0 in production |
| Crash-free sessions | > 99.5% |
| Network requests on main thread | 0 |
| Image load time | < 500ms (cached), < 2s (network) |

### LeakCanary Setup
```kotlin
// Add to debug build only
debugImplementation("com.squareup.leakcanary:leakcanary-android:2.x")
// No code changes needed — auto-detects leaks
```

---

## 6.6 Debugging Strategies

### Debug Tools
| Tool | Use Case |
|------|---------|
| Logcat | Real-time logs, filter by tag |
| Network Inspector | Inspect HTTP traffic in Android Studio |
| Database Inspector | Query Room DB live |
| Layout Inspector | Visual hierarchy, recomposition counts |
| Flipper (Facebook) | Advanced network/DB/layout debugging |
| Chucker | In-app HTTP inspector overlay |

### Common Debug Checklist
- [ ] Strict mode enabled in debug builds (detects main thread IO)
- [ ] LeakCanary installed in debug variant
- [ ] OkHttp logging interceptor enabled in debug only
- [ ] All hardcoded test data guarded with `BuildConfig.DEBUG`
- [ ] Debug menu / developer options screen for testers
