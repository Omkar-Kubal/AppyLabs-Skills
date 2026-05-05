# Flutter — Beyond UI Reference

Source: https://docs.flutter.dev (Beyond UI section)

## Table of Contents
1. [Data & Backend](#1-data--backend)
2. [App Architecture](#2-app-architecture)
3. [Platform Integration](#3-platform-integration)
4. [Packages & Plugins](#4-packages--plugins)
5. [Testing & Debugging](#5-testing--debugging)
6. [Performance & Optimization](#6-performance--optimization)
7. [Deployment](#7-deployment)
8. [Add to Existing App](#8-add-to-existing-app)

---

## 1. Data & Backend

### State Management

| Page | URL |
|---|---|
| Introduction | https://docs.flutter.dev/data-and-backend/state-mgmt/intro |
| Think declaratively | https://docs.flutter.dev/data-and-backend/state-mgmt/declarative |
| Ephemeral vs app state | https://docs.flutter.dev/data-and-backend/state-mgmt/ephemeral-vs-app |
| Simple app state management | https://docs.flutter.dev/data-and-backend/state-mgmt/simple |
| Options (all approaches) | https://docs.flutter.dev/data-and-backend/state-mgmt/options |

**State types:**
- **Ephemeral (local)** — lives in `State`, rebuilt with `setState()`. Use for: animation state, selected tab, form text.
- **App state (shared)** — needed across widgets/screens. Use a state management solution.

**Official recommendation:** Provider (simple) → Riverpod (advanced) → Bloc/Cubit (enterprise/team). See full comparison at options page.

```dart
// Provider pattern (simplest shared state)
// 1. Define model
class Counter extends ChangeNotifier {
  int value = 0;
  void increment() { value++; notifyListeners(); }
}
// 2. Provide
ChangeNotifierProvider(create: (_) => Counter(), child: MyApp())
// 3. Consume
Consumer<Counter>(builder: (ctx, counter, _) => Text('${counter.value}'))
// or:
context.watch<Counter>().value  // rebuilds on change
context.read<Counter>().increment()  // no rebuild
```

### Networking & HTTP

| Page | URL |
|---|---|
| Overview | https://docs.flutter.dev/data-and-backend/networking |
| Fetch data from internet | https://docs.flutter.dev/cookbook/networking/fetch-data |
| Authenticated requests | https://docs.flutter.dev/cookbook/networking/authenticated-requests |
| Send data (POST) | https://docs.flutter.dev/cookbook/networking/send-data |
| Update data (PUT) | https://docs.flutter.dev/cookbook/networking/update-data |
| Delete data (DELETE) | https://docs.flutter.dev/cookbook/networking/delete-data |
| WebSockets | https://docs.flutter.dev/cookbook/networking/web-sockets |

```dart
// HTTP fetch pattern (using http package)
import 'package:http/http.dart' as http;
import 'dart:convert';

Future<Post> fetchPost(int id) async {
  final response = await http.get(Uri.parse('https://jsonplaceholder.typicode.com/posts/$id'));
  if (response.statusCode == 200) return Post.fromJson(jsonDecode(response.body));
  throw Exception('Failed to load post');
}

// WebSocket
final channel = WebSocketChannel.connect(Uri.parse('wss://echo.websocket.org'));
channel.stream.listen((message) => print(message));
channel.sink.add('Hello');
```

### Serialization

| Page | URL |
|---|---|
| JSON serialization | https://docs.flutter.dev/data-and-backend/serialization/json |
| Parse JSON in background | https://docs.flutter.dev/cookbook/networking/background-parsing |

**Approaches:**
- **Manual:** `jsonDecode` + `fromJson`/`toJson` methods — simple, no codegen
- **Code generation:** `json_serializable` + `build_runner` — recommended for large models
- **Background:** use `compute(parseFunction, jsonString)` for large JSON to avoid jank

```dart
// json_serializable pattern
@JsonSerializable()
class User {
  final String name;
  final int age;
  User({required this.name, required this.age});
  factory User.fromJson(Map<String, dynamic> json) => _$UserFromJson(json);
  Map<String, dynamic> toJson() => _$UserToJson(this);
}
// Run: dart run build_runner build
```

### Persistence

| Page | URL |
|---|---|
| Key-value storage | https://docs.flutter.dev/cookbook/persistence/key-value |
| Read and write files | https://docs.flutter.dev/cookbook/persistence/reading-writing-files |
| SQLite | https://docs.flutter.dev/cookbook/persistence/sqlite |

**Options:**
- `shared_preferences` — key-value, primitives only, async, platform storage
- `path_provider` + `dart:io File` — arbitrary file I/O
- `sqflite` — SQLite, full CRUD, transactions
- `drift` (formerly moor) — type-safe SQLite ORM with codegen
- `hive` / `isar` — NoSQL, fast, no setup

### Firebase

| Page | URL |
|---|---|
| Overview | https://docs.flutter.dev/data-and-backend/firebase |
| Add Firebase to Flutter app | https://firebase.google.com/docs/flutter/setup |

**FlutterFire packages:** `firebase_core`, `firebase_auth`, `cloud_firestore`, `firebase_storage`, `firebase_messaging`, `firebase_analytics`, `firebase_crashlytics`.

Setup: `flutterfire configure` (FlutterFire CLI) auto-generates `firebase_options.dart`.

### Google APIs
**URL:** https://docs.flutter.dev/data-and-backend/google-apis
Use `googleapis` package. Auth via `google_sign_in` + `extension_google_sign_in_as_googleapis_auth`.

---

## 2. App Architecture

| Page | URL |
|---|---|
| Introduction | https://docs.flutter.dev/app-architecture |
| Architecture concepts | https://docs.flutter.dev/app-architecture/concepts |
| Guide to app architecture | https://docs.flutter.dev/app-architecture/guide |
| Case study: Overview | https://docs.flutter.dev/app-architecture/case-study |
| Case study: UI layer | https://docs.flutter.dev/app-architecture/case-study/ui-layer |
| Case study: Data layer | https://docs.flutter.dev/app-architecture/case-study/data-layer |
| Case study: Dependency injection | https://docs.flutter.gov/app-architecture/case-study/dependency-injection |
| Case study: Testing each layer | https://docs.flutter.dev/app-architecture/case-study/testing |
| Recommendations | https://docs.flutter.dev/app-architecture/recommendations |
| Design patterns | https://docs.flutter.dev/app-architecture/design-patterns |

### Recommended layered architecture

```
UI Layer          ← widgets + ViewModels/Controllers
  ↕
Domain Layer      ← use cases, business logic (optional for simple apps)
  ↕
Data Layer        ← repositories + data sources (API, DB, local)
```

**UI layer:** Widgets observe state (via ChangeNotifier, Riverpod, Bloc). ViewModels hold UI state, call repositories.

**Data layer:** Repository pattern — single source of truth. Repository coordinates between remote (HTTP) and local (DB/cache) data sources.

**Dependency injection:** Use `get_it` (service locator) or `provider`/`riverpod` for DI. Constructor injection preferred for testability.

**Design patterns covered:**
- Repository pattern
- Command pattern (for undo/redo, async operations)
- Result type pattern (instead of try/catch everywhere)
- Offline-first

---

## 3. Platform Integration

### Cross-platform

| Page | URL |
|---|---|
| Supported platforms | https://docs.flutter.dev/reference/supported-platforms |
| Build desktop apps | https://docs.flutter.dev/platform-integration/desktop |
| Platform-specific code (MethodChannel) | https://docs.flutter.dev/platform-integration/platform-channels |
| Bind to native code (FFI) | https://docs.flutter.dev/platform-integration/bind-native-code |

**MethodChannel pattern:**
```dart
// Dart side
const channel = MethodChannel('com.example/battery');
final int battery = await channel.invokeMethod('getBatteryLevel');

// Android (Kotlin)
MethodChannel(flutterEngine.dartExecutor.binaryMessenger, "com.example/battery")
  .setMethodCallHandler { call, result ->
    if (call.method == "getBatteryLevel") result.success(getBatteryLevel())
    else result.notImplemented()
  }
```

**FFI (dart:ffi):** Direct C interop. Use `ffigen` to generate bindings from C headers.

### Android

| Page | URL |
|---|---|
| Setup | https://docs.flutter.dev/platform-integration/android/setup |
| Splash screen | https://docs.flutter.dev/platform-integration/android/splash-screen |
| Predictive back | https://docs.flutter.dev/platform-integration/android/predictive-back |
| Platform views (AndroidView) | https://docs.flutter.dev/platform-integration/android/platform-views |
| Calling JetPack APIs | https://docs.flutter.dev/platform-integration/android/call-jetpack-apis |
| Launch Jetpack Compose activity | https://docs.flutter.dev/platform-integration/android/compose-activity |
| Restore state on Android | https://docs.flutter.dev/platform-integration/android/restore-state-android |
| Target ChromeOS | https://docs.flutter.dev/platform-integration/android/chromeos |
| Protect sensitive content | https://docs.flutter.dev/platform-integration/android/sensitive-content |

Key notes:
- Min SDK: Flutter requires minSdk ≥ 21 (Android 5.0) by default
- `FlutterActivity` / `FlutterFragment` as entry points
- Gradle config in `android/app/build.gradle`
- Predictive back: register `OnBackInvokedCallback` via Flutter plugin or directly

### iOS

| Page | URL |
|---|---|
| Setup | https://docs.flutter.dev/platform-integration/ios/setup |
| Flutter on latest iOS | https://docs.flutter.dev/platform-integration/ios/ios-latest |
| Apple system libraries | https://docs.flutter.dev/platform-integration/ios/apple-frameworks |
| Launch screen | https://docs.flutter.dev/platform-integration/ios/launch-screen |
| App Clip support | https://docs.flutter.dev/platform-integration/ios/ios-app-clip |
| App extensions | https://docs.flutter.dev/platform-integration/ios/app-extensions |
| Platform views (UiKitView) | https://docs.flutter.dev/platform-integration/ios/platform-views |
| Enable iOS debugging | https://docs.flutter.dev/platform-integration/ios/ios-debugging |
| Restore state on iOS | https://docs.flutter.dev/platform-integration/ios/restore-state-ios |

Key notes:
- Min iOS: 12 (Flutter 3.x)
- `FlutterViewController` as entry point
- Swift Package Manager supported (see Packages section)
- Info.plist for permissions (camera, location, etc.)

### Web

| Page | URL |
|---|---|
| Web support overview | https://docs.flutter.dev/platform-integration/web |
| Setup | https://docs.flutter.dev/platform-integration/web/setup |
| Web dev config file | https://docs.flutter.dev/platform-integration/web/web-dev-config-file |
| Build a web app | https://docs.flutter.dev/platform-integration/web/building |
| Compile to WebAssembly (Wasm) | https://docs.flutter.dev/platform-integration/web/wasm |
| Customize app initialization | https://docs.flutter.dev/platform-integration/web/initialization |
| Embed Flutter in any web app | https://docs.flutter.dev/platform-integration/web/embedding-flutter-web |
| Web content in Flutter | https://docs.flutter.dev/platform-integration/web/web-content-in-flutter |
| Web renderers | https://docs.flutter.dev/platform-integration/web/renderers |
| Display images on web | https://docs.flutter.dev/platform-integration/web/web-images |
| Web FAQ | https://docs.flutter.dev/platform-integration/web/faq |

Key notes:
- Renderers: **CanvasKit** (default, Wasm-based, pixel-perfect) vs **HTML** (smaller, less faithful)
- **Wasm:** `flutter build web --wasm` — requires Chrome 119+, best performance
- URL strategies: hash (`/#/`) vs path (`/`) — configure for web
- `flutter_web_plugins` for web-specific implementations

### macOS

| Page | URL |
|---|---|
| Setup | https://docs.flutter.dev/platform-integration/macos/setup |
| Build a macOS app | https://docs.flutter.dev/platform-integration/macos/building |
| Platform views | https://docs.flutter.dev/platform-integration/macos/platform-views |

### Linux

| Page | URL |
|---|---|
| Setup | https://docs.flutter.dev/platform-integration/linux/setup |
| Build a Linux app | https://docs.flutter.dev/platform-integration/linux/building |

### Windows

| Page | URL |
|---|---|
| Setup | https://docs.flutter.dev/platform-integration/windows/setup |
| Build a Windows app | https://docs.flutter.dev/platform-integration/windows/building |

---

## 4. Packages & Plugins

| Page | URL |
|---|---|
| Use packages & plugins | https://docs.flutter.dev/packages-and-plugins/using-packages |
| Develop packages & plugins | https://docs.flutter.dev/packages-and-plugins/developing-packages |
| Swift Package Manager — app devs | https://docs.flutter.dev/packages-and-plugins/swift-package-manager/for-app-developers |
| Swift Package Manager — plugin authors | https://docs.flutter.dev/packages-and-plugins/swift-package-manager/for-plugin-authors |
| Flutter Favorites | https://docs.flutter.dev/packages-and-plugins/favorites |
| Package repository | https://pub.dev/flutter |

### Package types
- **Dart package** — pure Dart, no platform code
- **Plugin package** — platform-specific implementations (Kotlin/Swift/JS)
- **FFI plugin** — native code via dart:ffi

### Essential packages (community standard)
| Purpose | Package |
|---|---|
| HTTP | `http`, `dio` |
| Routing | `go_router` (official) |
| State mgmt | `provider`, `riverpod`, `flutter_bloc` |
| DI | `get_it` |
| JSON codegen | `json_serializable` + `build_runner` |
| Local DB | `sqflite`, `drift`, `isar` |
| Key-value | `shared_preferences` |
| Image loading | `cached_network_image` |
| SVG | `flutter_svg` |
| Fonts | `google_fonts` |
| Testing mocks | `mockito`, `mocktail` |
| Firebase | `firebase_core` + FlutterFire suite |

---

## 5. Testing & Debugging

### Testing

| Page | URL |
|---|---|
| Testing overview | https://docs.flutter.dev/testing/overview |
| Unit test introduction | https://docs.flutter.dev/cookbook/testing/unit/introduction |
| Mock dependencies | https://docs.flutter.dev/cookbook/testing/unit/mocking |
| Widget test introduction | https://docs.flutter.dev/cookbook/testing/widget/introduction |
| Find widgets | https://docs.flutter.dev/cookbook/testing/widget/finders |
| Simulate scrolling | https://docs.flutter.dev/cookbook/testing/widget/scrolling |
| Simulate user interaction | https://docs.flutter.dev/cookbook/testing/widget/tap-drag |
| Integration test introduction | https://docs.flutter.dev/cookbook/testing/integration/introduction |
| Write & run integration test | https://docs.flutter.dev/testing/integration-tests |
| Profile integration test | https://docs.flutter.dev/cookbook/testing/integration/profiling |
| Test a plugin | https://docs.flutter.dev/testing/testing-plugins |
| Handle plugin code in tests | https://docs.flutter.dev/testing/plugins-in-tests |

### Test types
```dart
// Unit test
test('Counter increments', () {
  final counter = Counter();
  counter.increment();
  expect(counter.value, 1);
});

// Widget test
testWidgets('Button shows text', (WidgetTester tester) async {
  await tester.pumpWidget(MaterialApp(home: MyWidget()));
  expect(find.text('Press me'), findsOneWidget);
  await tester.tap(find.byType(ElevatedButton));
  await tester.pump(); // trigger rebuild
  expect(find.text('Pressed!'), findsOneWidget);
});

// Finders: find.text(), find.byType(), find.byKey(), find.byIcon(), find.descendant()
```

### Debugging

| Page | URL |
|---|---|
| Debugging tools | https://docs.flutter.dev/testing/debugging |
| Debug programmatically | https://docs.flutter.dev/testing/code-debugging |
| Native language debugger | https://docs.flutter.dev/testing/native-debugging |
| Common Flutter errors | https://docs.flutter.dev/testing/common-errors |
| Handle errors | https://docs.flutter.dev/testing/errors |
| Report errors to service | https://docs.flutter.dev/cookbook/maintenance/error-reporting |

**Programmatic debugging:**
```dart
debugPrint('message');            // throttled print
debugDumpApp();                   // dump widget tree
debugDumpRenderTree();            // dump render tree
debugPaintSizeEnabled = true;     // visualize layout bounds
debugRepaintRainbowEnabled = true; // visualize repaints
FlutterError.onError = (details) { /* custom error handling */ };
```

**Common errors:**
- `RenderFlex overflowed` → wrap in `Expanded`, `Flexible`, or `SingleChildScrollView`
- `setState called after dispose` → check `mounted` before setState
- `Null check operator on null` → null safety violation, audit `!` usage
- `A build function returned null` → widget must return a widget, never null

---

## 6. Performance & Optimization

| Page | URL |
|---|---|
| Overview | https://docs.flutter.dev/perf |
| Impeller (rendering engine) | https://docs.flutter.dev/perf/impeller |
| Performance best practices | https://docs.flutter.dev/perf/best-practices |
| App size | https://docs.flutter.dev/perf/app-size |
| Deferred components | https://docs.flutter.dev/perf/deferred-components |
| Rendering performance | https://docs.flutter.dev/perf/rendering-performance |
| Performance profiling (DevTools) | https://docs.flutter.dev/perf/ui-performance |
| Web performance profiling | https://docs.flutter.dev/perf/web-performance |
| Performance metrics | https://docs.flutter.dev/perf/metrics |
| Concurrency and isolates | https://docs.flutter.dev/perf/isolates |
| Performance FAQ | https://docs.flutter.dev/perf/faq |
| Appendix | https://docs.flutter.dev/perf/appendix |

### Impeller
Flutter's new rendering engine (replaces Skia). Default on iOS (Flutter 3.10+), Android (Flutter 3.16+). Eliminates shader compilation jank. Disable with `--no-enable-impeller` for debugging.

### Best practices
- Use `const` constructors everywhere possible — skips rebuild
- Avoid `setState` high in the tree — scope to smallest widget
- `ListView.builder` not `ListView(children: [...])` for long lists — lazy building
- `RepaintBoundary` — isolate frequently-repainting widgets
- Avoid `Opacity` widget animating — use `FadeTransition` (GPU-only)
- Cache expensive computations; avoid in `build()`
- `saveLayer()` (used by Opacity, ShaderMask, BackdropFilter) is expensive

### Isolates (concurrency)
```dart
// compute() — run function in isolate, returns Future
final result = await compute(parseHeavyJson, rawJsonString);

// Isolate.run() (Dart 2.19+) — simpler API
final result = await Isolate.run(() => expensiveOperation(data));

// Long-lived isolate with SendPort/ReceivePort for streaming work
```

### App size
- `flutter build apk --split-per-abi` — separate APKs per ABI, smaller per device
- `flutter build appbundle` — Play Store dynamic delivery, smallest download
- Analyze: `flutter build apk --analyze-size` → opens DevTools size view
- Deferred loading: `deferred as` keyword + `loadLibrary()` for lazy imports

---

## 7. Deployment

| Page | URL |
|---|---|
| Obfuscate Dart code | https://docs.flutter.dev/deployment/obfuscate |
| App flavors (Android) | https://docs.flutter.dev/deployment/flavors |
| App flavors (iOS/macOS) | https://docs.flutter.dev/deployment/flavors-ios |
| Build & release Android app | https://docs.flutter.dev/deployment/android |
| Build & release iOS app | https://docs.flutter.dev/deployment/ios |
| Build & release macOS app | https://docs.flutter.dev/deployment/macos |
| Build & release Linux app | https://docs.flutter.dev/deployment/linux |
| Build & release Windows app | https://docs.flutter.dev/deployment/windows |
| Build & release web app | https://docs.flutter.dev/deployment/web |
| Set up continuous deployment | https://docs.flutter.dev/deployment/cd |

### Android release checklist
1. Sign app: `keytool -genkey` → `key.properties` → configure in `build.gradle`
2. `flutter build appbundle` (preferred for Play Store)
3. Or `flutter build apk --release` (direct APK)
4. Enable R8/ProGuard shrinking in `build.gradle`
5. Obfuscate: `flutter build appbundle --obfuscate --split-debug-info=/<dir>/`

### iOS release checklist
1. Xcode: set Bundle ID, signing team
2. `flutter build ipa`
3. Upload via Xcode Organizer or `xcrun altool`
4. TestFlight → App Store review

### Flavors (dev/staging/prod)
```dart
// Dart side — read flavor
const flavor = String.fromEnvironment('FLUTTER_APP_FLAVOR');
// Run: flutter run --dart-define=FLUTTER_APP_FLAVOR=dev
```

### CI/CD
Tools: Fastlane, Codemagic, GitHub Actions, Bitrise. `flutter test` + `flutter build` as core steps.

---

## 8. Add to Existing App

| Page | URL |
|---|---|
| Introduction | https://docs.flutter.dev/add-to-app |
| Android: Project setup | https://docs.flutter.dev/add-to-app/android/project-setup |
| Android: Add single Flutter screen | https://docs.flutter.dev/add-to-app/android/add-flutter-screen |
| Android: Add Flutter Fragment | https://docs.flutter.dev/add-to-app/android/add-flutter-fragment |
| Android: Add Flutter View | https://docs.flutter.dev/add-to-app/android/add-flutter-view |
| Android: Use Flutter plugin | https://docs.flutter.dev/add-to-app/android/plugin-setup |
| iOS: Project setup | https://docs.flutter.dev/add-to-app/ios/project-setup |
| iOS: Add single Flutter screen | https://docs.flutter.dev/add-to-app/ios/add-flutter-screen |
| Add to web app | https://docs.flutter.dev/platform-integration/web/embedding-flutter-web |
| Debug embedded Flutter module | https://docs.flutter.dev/add-to-app/debugging |
| Multiple Flutter instances | https://docs.flutter.dev/add-to-app/multiple-flutters |
| Loading sequence & performance | https://docs.flutter.dev/add-to-app/performance |

### Key concepts
- Create a **Flutter module**: `flutter create --template module my_flutter`
- Android: add as Gradle dependency (source or AAR)
- iOS: add via CocoaPods or Swift Package Manager
- `FlutterEngine` must be pre-warmed for fast display (avoid cold start on first screen)
- **Multiple engines:** Use `FlutterEngineGroup` to share resources across instances
- Platform ↔ Flutter communication: same `MethodChannel` / `EventChannel` approach

```kotlin
// Android — pre-warm engine
class MyApp : Application() {
  val flutterEngine by lazy {
    FlutterEngine(this).also {
      it.dartExecutor.executeDartEntrypoint(DartExecutor.DartEntrypoint.createDefault())
      FlutterEngineCache.getInstance().put("my_engine_id", it)
    }
  }
  override fun onCreate() { super.onCreate(); flutterEngine }
}
```
