# Flutter — Resources Reference

Source: https://docs.flutter.dev (Resources section)

## Table of Contents
1. [Tools & Editors](#1-tools--editors)
2. [DevTools](#2-devtools)
3. [Flutter Concepts](#3-flutter-concepts)
4. [Migration Guides](#4-migration-guides)
5. [Releases](#5-releases)
6. [Cookbook](#6-cookbook)
7. [AI & Flutter](#7-ai--flutter)

---

## 1. Tools & Editors

| Page | URL |
|---|---|
| Android Studio & IntelliJ | https://docs.flutter.dev/tools/android-studio |
| Visual Studio Code | https://docs.flutter.dev/tools/vs-code |
| Antigravity (experimental) | https://docs.flutter.dev/tools/antigravity |
| Widget Previewer | https://docs.flutter.dev/tools/widget-previewer |
| Property Editor | https://docs.flutter.dev/tools/property-editor |
| SDK overview | https://docs.flutter.dev/tools/sdk |
| Flutter's pubspec options | https://docs.flutter.dev/tools/pubspec |
| Automated fixes (flutter fix) | https://docs.flutter.dev/tools/flutter-fix |
| Code formatting | https://docs.flutter.dev/tools/formatting |
| Hot reload | https://docs.flutter.dev/tools/hot-reload |

### Key CLI commands
```bash
# Setup & info
flutter doctor                  # check environment
flutter --version               # SDK version
flutter upgrade                 # upgrade Flutter SDK
flutter channel [stable|beta|main]  # switch channel

# Project
flutter create my_app           # new app
flutter create --template module my_module   # add-to-app module
flutter create --template plugin my_plugin   # plugin

# Run & build
flutter run                     # run on connected device
flutter run -d chrome           # run on web
flutter run --release           # release mode
flutter build apk               # Android APK
flutter build appbundle         # Android App Bundle
flutter build ios               # iOS (requires Mac)
flutter build web               # web
flutter build web --wasm        # web with Wasm

# Test
flutter test                    # run all unit + widget tests
flutter test integration_test/  # integration tests
flutter test --coverage         # with coverage report

# Analysis & formatting
flutter analyze                 # static analysis (dart analyze)
dart format .                   # format all Dart files
flutter fix --apply             # apply automated fixes

# Packages
flutter pub get                 # fetch dependencies
flutter pub upgrade             # upgrade dependencies
flutter pub outdated            # check outdated packages
dart pub publish                # publish package to pub.dev

# Performance & size
flutter build apk --analyze-size  # size analysis
flutter build apk --split-per-abi  # per-ABI APKs
flutter build appbundle --obfuscate --split-debug-info=./debug-info

# DevTools
dart devtools                   # launch DevTools in browser
flutter run --profile           # profile mode for DevTools perf
```

### pubspec.yaml key fields
```yaml
name: my_app
description: A Flutter app
version: 1.0.0+1              # version+build_number

environment:
  sdk: '>=3.0.0 <4.0.0'

dependencies:
  flutter:
    sdk: flutter
  # pub.dev package:
  http: ^1.1.0
  # Git dependency:
  my_pkg:
    git:
      url: https://github.com/org/repo.git
      ref: main
  # Local path:
  my_local:
    path: ../my_local_package

dev_dependencies:
  flutter_test:
    sdk: flutter
  build_runner: ^2.4.0
  json_serializable: ^6.7.0

flutter:
  uses-material-design: true
  generate: true             # enable l10n generation
  assets:
    - assets/images/
  fonts:
    - family: MyFont
      fonts:
        - asset: fonts/MyFont-Regular.ttf
```

### Code formatting & analysis
- `dart format .` — enforces Dart style (replaces `dartfmt`)
- `analysis_options.yaml` — configure lints; use `flutter_lints` package (default)
- `flutter fix --apply` — auto-migrate deprecated APIs
- `dart fix --apply` — broader Dart fixes

---

## 2. DevTools

**URL:** https://docs.flutter.dev/tools/devtools
**Launch:** `dart devtools` or via IDE extension

### DevTools views — all 17

| View | URL | Purpose |
|---|---|---|
| Overview | https://docs.flutter.dev/tools/devtools | What DevTools is |
| Run from Android Studio | https://docs.flutter.dev/tools/devtools/android-studio | IDE integration |
| Run from VS Code | https://docs.flutter.dev/tools/devtools/vscode | IDE integration |
| Run from CLI | https://docs.flutter.dev/tools/devtools/cli | Standalone |
| **Flutter Inspector** | https://docs.flutter.dev/tools/devtools/inspector | Widget tree, layout explorer, select widget on device |
| Legacy Inspector | https://docs.flutter.dev/tools/devtools/legacy-inspector | Old inspector UI |
| **Performance view** | https://docs.flutter.dev/tools/devtools/performance | Frame timeline, jank detection, shader cache |
| **CPU Profiler** | https://docs.flutter.dev/tools/devtools/cpu-profiler | CPU flame chart, call tree, bottom-up |
| **Memory view** | https://docs.flutter.dev/tools/devtools/memory | Heap snapshots, memory leaks, allocation tracing |
| Debug console | https://docs.flutter.dev/tools/devtools/console | Evaluate Dart expressions at runtime |
| **Network view** | https://docs.flutter.dev/tools/devtools/network | HTTP/HTTPS request inspection |
| **Debugger** | https://docs.flutter.dev/tools/devtools/debugger | Breakpoints, step through, variables |
| Logging view | https://docs.flutter.dev/tools/devtools/logging | `debugPrint`, `log()` output |
| **App size tool** | https://docs.flutter.dev/tools/devtools/app-size | Binary size breakdown, tree map |
| DevTools extensions | https://docs.flutter.dev/tools/devtools/extensions | Third-party DevTools panels (Riverpod, Bloc, etc.) |
| Build a custom tool | https://docs.flutter.dev/tools/devtools/custom-tool | Create custom DevTools extension |
| **Deep links validator** | https://docs.flutter.dev/tools/devtools/deep-links | Validate Android App Links / iOS Universal Links |
| Release notes | https://docs.flutter.dev/tools/devtools/release-notes | DevTools changelog |

### Performance debugging workflow
1. Run in **profile** mode: `flutter run --profile`
2. Open DevTools → **Performance** view
3. Record — perform the action causing jank
4. Find frames taking >16ms (60fps) or >8ms (120fps)
5. Inspect timeline events: UI thread vs Raster thread
6. If Raster thread jank → shader compilation (Impeller solves this) or expensive painting
7. If UI thread jank → expensive `build()` calls, heavy computation on main isolate

### Memory debugging workflow
1. Open DevTools → **Memory** view
2. Take heap snapshot before and after suspected leak
3. Compare — look for objects that grew and weren't GC'd
4. Use **Allocation Tracing** to find where objects are allocated

### Inspector key features
- **Select Widget Mode** — tap widget on device → highlights in tree
- **Layout Explorer** — visualize Flex/Row/Column constraints and sizes
- **Widget Details Tree** — see all properties of selected widget

---

## 3. Flutter Concepts

| Page | URL |
|---|---|
| Architectural overview | https://docs.flutter.dev/resources/architectural-overview |
| Inside Flutter | https://docs.flutter.dev/resources/inside-flutter |
| Understanding constraints | https://docs.flutter.dev/ui/layout/constraints |
| Flutter's build modes | https://docs.flutter.dev/testing/build-modes |
| Hot reload | https://docs.flutter.dev/tools/hot-reload |

### Flutter architecture layers (top → bottom)
```
┌─────────────────────────────────────┐
│  Framework (Dart)                   │
│  Material / Cupertino               │
│  Widgets                            │
│  Rendering                          │
│  Animation, Painting, Gestures      │
│  Foundation                         │
├─────────────────────────────────────┤
│  Engine (C++)                       │
│  Skia / Impeller (graphics)         │
│  Dart runtime                       │
│  Text, Platform channels            │
├─────────────────────────────────────┤
│  Embedder (platform-specific)       │
│  iOS / Android / Web / Desktop      │
└─────────────────────────────────────┘
```

### Widget → Element → RenderObject pipeline
- **Widget** — immutable config/description (cheap to create)
- **Element** — mutable, live instance, holds reference to widget + render object; manages lifecycle
- **RenderObject** — handles layout, painting, hit testing (expensive; reused)
- Flutter diffs widget tree → updates only changed elements → minimal RenderObject updates

### Three trees reconciliation
- On `setState`: Flutter calls `build()`, creates new widget subtree
- Element tree reconciles: same type at same position → update (cheap); different type → replace (creates new element + render object)
- `Key` forces element identity across position changes: `ValueKey`, `ObjectKey`, `UniqueKey`, `GlobalKey`

### Keys — when and why
```dart
// Use keys when reordering stateful widgets
ListView(children: items.map((i) => ListTile(key: ValueKey(i.id), title: Text(i.name))).toList())

// GlobalKey — access State from outside widget
final key = GlobalKey<FormState>();
key.currentState!.validate();
```

### Inside Flutter (advanced)
- **Linear reconciliation** — O(n) element diffing (not tree diffing like React's O(n³))
- **Sublinear layout** — single-pass layout via constraints propagation (no relayout cycles)
- **Render pipeline steps:** animate → build → layout → compositing bits → paint → composite
- `markNeedsBuild()` / `markNeedsLayout()` / `markNeedsPaint()` — schedule individual stages

### Constraints deep dive
Rule: **Tight** constraints (min==max) force exact size. **Loose** constraints (min==0) let child choose.
- `SizedBox` — forces tight constraint on child
- `Center` / `Align` — passes loose constraint (child can be smaller than parent)
- `Expanded` — forces tight constraint filling remaining flex space
- `UnconstrainedBox` — removes constraints (child can overflow — use cautiously)
- `OverflowBox` — allows child to exceed parent size

---

## 4. Migration Guides

Key migrations — check https://docs.flutter.dev/release/breaking-changes for full list.

| Migration | URL |
|---|---|
| Migrate to Material 3 | https://docs.flutter.dev/release/breaking-changes/material-3-migration |
| All breaking changes | https://docs.flutter.dev/release/breaking-changes |
| Flutter 3.x migration index | https://docs.flutter.dev/release/migration-guide |

### Material 3 migration highlights
```dart
// Before (M2)
ThemeData(primarySwatch: Colors.blue)

// After (M3)
ThemeData(
  useMaterial3: true,
  colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
)

// Renamed widgets: Chip → FilterChip/InputChip/ActionChip
// NavigationRail updated, NavigationBar replaces BottomNavigationBar for M3
// TextTheme properties renamed (headline6 → titleLarge, etc.)
```

### Common migration tasks
- `flutter fix --apply` — handles many mechanical API renames automatically
- Check `dart analyze` after upgrading — breaking changes surface as errors
- Use `dart pub upgrade --major-versions` to update package constraints

---

## 5. Releases

| Resource | URL |
|---|---|
| Release notes | https://docs.flutter.dev/release/release-notes |
| Breaking changes | https://docs.flutter.dev/release/breaking-changes |
| Migration guide | https://docs.flutter.dev/release/migration-guide |
| Archive (older releases) | https://docs.flutter.dev/release/archive |

### Channels
- `stable` — quarterly releases, production use
- `beta` — monthly, pre-release of next stable
- `main` — continuous, cutting edge, unstable

Switch: `flutter channel stable && flutter upgrade`

---

## 6. Cookbook

Full cookbook index: https://docs.flutter.dev/cookbook

### Cookbook categories and key recipes

**Animation**
- Animate a page route transition: https://docs.flutter.dev/cookbook/animation/page-route-animation
- Animate a container: https://docs.flutter.dev/cookbook/animation/animated-container
- Physics simulation: https://docs.flutter.dev/cookbook/animation/physics-simulation
- Staggered menu: https://docs.flutter.dev/cookbook/effects/staggered-menu-animation

**Design**
- Use themes: https://docs.flutter.dev/cookbook/design/themes
- Use custom fonts: https://docs.flutter.dev/cookbook/design/fonts
- Add a drawer: https://docs.flutter.dev/cookbook/design/drawer
- Display a snackbar: https://docs.flutter.dev/cookbook/design/snackbars
- Add tabs: https://docs.flutter.dev/cookbook/design/tabs
- Cupertino sheets: https://docs.flutter.dev/cookbook/design/cupertino-sheets

**Effects**
- Drag a widget: https://docs.flutter.dev/cookbook/effects/drag-a-widget
- Expandable FAB: https://docs.flutter.dev/cookbook/effects/expandable-fab
- Gradient chat bubbles: https://docs.flutter.dev/cookbook/effects/gradient-bubbles
- Shimmer loading effect: https://docs.flutter.dev/cookbook/effects/shimmer-loading
- Typing indicator: https://docs.flutter.dev/cookbook/effects/typing-indicator
- Parallax scrolling: https://docs.flutter.dev/cookbook/effects/parallax-scrolling

**Forms**
- Validation: https://docs.flutter.dev/cookbook/forms/validation
- Text input: https://docs.flutter.dev/cookbook/forms/text-input
- Focus management: https://docs.flutter.dev/cookbook/forms/focus

**Gestures**
- Handle taps: https://docs.flutter.dev/cookbook/gestures/handling-taps
- Swipe to dismiss: https://docs.flutter.dev/cookbook/gestures/dismissible
- Ripple effect: https://docs.flutter.dev/cookbook/gestures/ripples

**Images**
- Display network image: https://docs.flutter.dev/cookbook/images/network-image
- Fade in images: https://docs.flutter.dev/cookbook/images/fading-in-images

**Lists**
- Basic list: https://docs.flutter.dev/cookbook/lists/basic-list
- Long lists (lazy): https://docs.flutter.dev/cookbook/lists/long-lists
- Grid view: https://docs.flutter.dev/cookbook/lists/grid-lists
- Floating app bar: https://docs.flutter.dev/cookbook/lists/floating-app-bar

**Maintenance**
- Report errors: https://docs.flutter.dev/cookbook/maintenance/error-reporting

**Navigation**
- Navigation basics: https://docs.flutter.dev/cookbook/navigation/navigation-basics
- Pass data: https://docs.flutter.dev/cookbook/navigation/passing-data
- Return data: https://docs.flutter.dev/cookbook/navigation/returning-data
- Deep links Android: https://docs.flutter.dev/cookbook/navigation/set-up-app-links
- Deep links iOS: https://docs.flutter.dev/cookbook/navigation/set-up-universal-links

**Networking**
- Fetch data: https://docs.flutter.dev/cookbook/networking/fetch-data
- Authenticated requests: https://docs.flutter.dev/cookbook/networking/authenticated-requests
- WebSockets: https://docs.flutter.dev/cookbook/networking/web-sockets
- Background JSON parsing: https://docs.flutter.dev/cookbook/networking/background-parsing

**Persistence**
- Key-value (SharedPreferences): https://docs.flutter.dev/cookbook/persistence/key-value
- Files: https://docs.flutter.dev/cookbook/persistence/reading-writing-files
- SQLite: https://docs.flutter.dev/cookbook/persistence/sqlite

**Plugins**
- Play video: https://docs.flutter.dev/cookbook/plugins/play-video
- Camera: https://docs.flutter.dev/cookbook/plugins/picture-using-camera

**Testing**
- Unit tests: https://docs.flutter.dev/cookbook/testing/unit/introduction
- Mocking: https://docs.flutter.dev/cookbook/testing/unit/mocking
- Widget tests: https://docs.flutter.dev/cookbook/testing/widget/introduction
- Integration tests: https://docs.flutter.dev/cookbook/testing/integration/introduction

---

## 7. AI & Flutter

**URL:** https://docs.flutter.dev/ai/create-with-ai

### flutter_ai_toolkit (official)
Package providing ready-made AI chat UI widgets + provider abstraction.
- `GeminiProvider` — Google Gemini integration
- `VertexProvider` — Vertex AI (Firebase)
- `LlmChatView` — drop-in chat widget
- Supports: streaming, history, custom styling, file/image attachments

```dart
// Gemini integration
import 'package:flutter_ai_toolkit/flutter_ai_toolkit.dart';
import 'package:google_generative_ai/google_generative_ai.dart';

LlmChatView(
  provider: GeminiProvider(
    model: GenerativeModel(model: 'gemini-2.0-flash', apiKey: 'YOUR_KEY'),
  ),
)
```

**Related packages:**
- `google_generative_ai` — Gemini SDK for Dart
- `firebase_vertexai` — Vertex AI via Firebase (recommended for production — no API key in client)
