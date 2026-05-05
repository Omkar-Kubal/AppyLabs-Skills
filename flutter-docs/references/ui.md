# Flutter — User Interface Reference

Source: https://docs.flutter.dev/ui

## Table of Contents
1. [UI Introduction](#1-ui-introduction)
2. [Widget Catalog](#2-widget-catalog)
3. [Layout](#3-layout)
4. [Adaptive & Responsive Design](#4-adaptive--responsive-design)
5. [Design & Theming](#5-design--theming)
6. [Interactivity](#6-interactivity)
7. [Assets & Media](#7-assets--media)
8. [Navigation & Routing](#8-navigation--routing)
9. [Animations & Transitions](#9-animations--transitions)
10. [Accessibility](#10-accessibility)
11. [Internationalization](#11-internationalization)

---

## 1. UI Introduction
**URL:** https://docs.flutter.dev/ui

Key concepts:
- Flutter renders its own widgets via Skia/Impeller — no platform UI components used
- Widget tree → Element tree → Render tree pipeline
- `MaterialApp` or `CupertinoApp` as root; wraps Navigator, Theme, Localizations
- `Scaffold` provides standard page structure: AppBar, body, FAB, Drawer, BottomNav
- `BuildContext` — handle to widget's location in tree; used to look up ancestors (Theme, Navigator, etc.)

---

## 2. Widget Catalog

### Overview
**URL:** https://docs.flutter.dev/ui/widgets

### Design Systems

| System | URL | Use when |
|---|---|---|
| Material components | https://docs.flutter.dev/ui/widgets/material | Android-style or cross-platform M3 design |
| Cupertino | https://docs.flutter.dev/ui/widgets/cupertino | iOS-native look (CupertinoButton, CupertinoPicker, etc.) |

**Material 3 (M3):** Default since Flutter 3.16. Enable via `useMaterial3: true` in `ThemeData`.
Key M3 widgets: `NavigationBar`, `NavigationDrawer`, `SegmentedButton`, `Badge`, `Card` (filled/elevated/outlined).

**Cupertino key widgets:** `CupertinoApp`, `CupertinoNavigationBar`, `CupertinoTabScaffold`, `CupertinoAlertDialog`, `CupertinoActionSheet`, `CupertinoDatePicker`, `CupertinoSlidingSegmentedControl`.

### Base Widget Categories

| Category | URL | Key widgets |
|---|---|---|
| Accessibility | https://docs.flutter.dev/ui/widgets/accessibility | `Semantics`, `ExcludeSemantics`, `MergeSemantics` |
| Animation | https://docs.flutter.dev/ui/widgets/animation | `AnimatedBuilder`, `AnimatedWidget`, `Hero`, `FadeTransition` |
| Assets | https://docs.flutter.dev/ui/widgets/assets | `Image`, `Icon`, `AssetBundle` |
| Async | https://docs.flutter.dev/ui/widgets/async | `FutureBuilder`, `StreamBuilder` |
| Basics | https://docs.flutter.dev/ui/widgets/basics | `Container`, `Row`, `Column`, `Stack`, `Text`, `Scaffold`, `AppBar` |
| Input | https://docs.flutter.dev/ui/widgets/input | `TextField`, `TextFormField`, `Checkbox`, `Radio`, `Slider`, `Switch`, `DropdownButton` |
| Interaction | https://docs.flutter.dev/ui/widgets/interaction | `GestureDetector`, `InkWell`, `Draggable`, `DragTarget`, `Dismissible` |
| Layout | https://docs.flutter.dev/ui/widgets/layout | `Padding`, `Align`, `Center`, `Expanded`, `Flexible`, `SizedBox`, `ConstrainedBox`, `Wrap`, `Flow` |
| Painting | https://docs.flutter.dev/ui/widgets/painting | `DecoratedBox`, `CustomPaint`, `ClipRect`, `ClipRRect`, `ClipOval`, `ShaderMask`, `BackdropFilter` |
| Scrolling | https://docs.flutter.dev/ui/widgets/scrolling | `ListView`, `GridView`, `SingleChildScrollView`, `CustomScrollView`, `Sliver*` |
| Styling | https://docs.flutter.dev/ui/widgets/styling | `Theme`, `MediaQuery`, `DefaultTextStyle` |
| Text | https://docs.flutter.dev/ui/widgets/text | `Text`, `RichText`, `DefaultTextStyle`, `SelectableText` |

### Critical widget patterns
```dart
// FutureBuilder pattern
FutureBuilder<MyData>(
  future: fetchData(),
  builder: (context, snapshot) {
    if (snapshot.connectionState == ConnectionState.waiting) return CircularProgressIndicator();
    if (snapshot.hasError) return Text('Error: ${snapshot.error}');
    return Text(snapshot.data!.value);
  },
)

// StreamBuilder pattern
StreamBuilder<int>(
  stream: myStream,
  builder: (context, snapshot) => Text('${snapshot.data ?? 0}'),
)
```

---

## 3. Layout

### Pages
| Page | URL |
|---|---|
| Introduction | https://docs.flutter.dev/ui/layout |
| Build a layout (tutorial) | https://docs.flutter.dev/ui/layout/tutorial |
| Understanding constraints | https://docs.flutter.dev/ui/layout/constraints |
| Scrolling overview | https://docs.flutter.dev/ui/layout/scrolling |
| Slivers | https://docs.flutter.dev/ui/layout/scrolling/slivers |

### Lists & Grids (Cookbook)
| Topic | URL |
|---|---|
| Create and use lists | https://docs.flutter.dev/cookbook/lists/basic-list |
| Horizontal list | https://docs.flutter.dev/cookbook/lists/horizontal-list |
| Grid view | https://docs.flutter.dev/cookbook/lists/grid-lists |
| Mixed-type lists | https://docs.flutter.dev/cookbook/lists/mixed-list |
| Spaced items | https://docs.flutter.dev/cookbook/lists/spaced-items |
| Long lists (lazy loading) | https://docs.flutter.dev/cookbook/lists/long-lists |
| Floating app bar above list | https://docs.flutter.dev/cookbook/lists/floating-app-bar |
| Parallax scrolling | https://docs.flutter.dev/cookbook/effects/parallax-scrolling |

### Layout key concepts
- **Row/Column:** `mainAxisAlignment`, `crossAxisAlignment`, `mainAxisSize`
- **Expanded vs Flexible:** `Expanded` = `Flexible(fit: FlexFit.tight)` — fills remaining space. `Flexible` allows child to be smaller.
- **Stack:** overlays widgets. Use `Positioned` for absolute placement.
- **LayoutBuilder:** build widget based on parent constraints at runtime.
- **IntrinsicHeight/Width:** forces children to match each other's intrinsic dimension (expensive — avoid in scrolling lists).
- **Slivers:** composable scroll pieces. `SliverList`, `SliverGrid`, `SliverAppBar`, `SliverFillRemaining`. Use inside `CustomScrollView`.

```dart
// Sliver pattern
CustomScrollView(
  slivers: [
    SliverAppBar(pinned: true, expandedHeight: 200, flexibleSpace: FlexibleSpaceBar(...)),
    SliverList(delegate: SliverChildBuilderDelegate((ctx, i) => ListTile(title: Text('$i')), childCount: 50)),
  ],
)
```

---

## 4. Adaptive & Responsive Design

| Page | URL |
|---|---|
| Overview | https://docs.flutter.dev/ui/adaptive-responsive |
| General approach | https://docs.flutter.dev/ui/adaptive-responsive/general |
| SafeArea & MediaQuery | https://docs.flutter.dev/ui/adaptive-responsive/safearea-mediaquery |
| Large screens & foldables | https://docs.flutter.dev/ui/adaptive-responsive/large-screens |
| User input & accessibility | https://docs.flutter.dev/ui/adaptive-responsive/input |
| Capabilities & policies | https://docs.flutter.dev/ui/adaptive-responsive/capabilities |
| Automatic platform adaptations | https://docs.flutter.dev/ui/adaptive-responsive/platform-adaptations |
| Best practices | https://docs.flutter.dev/ui/adaptive-responsive/best-practices |
| Additional resources | https://docs.flutter.dev/ui/adaptive-responsive/more-info |

### Key APIs
- `MediaQuery.of(context).size` — screen dimensions
- `MediaQuery.of(context).padding` — system insets (notch, nav bar)
- `SafeArea` — auto-pads for system UI intrusions
- `LayoutBuilder` — responsive layout based on available space
- `AdaptiveScaffold` (flutter_adaptive_scaffold pkg) — breakpoint-aware layouts
- Platform detection: `Platform.isAndroid`, `Platform.isIOS`, `kIsWeb`, `Theme.of(context).platform`

```dart
// Responsive layout pattern
LayoutBuilder(
  builder: (context, constraints) {
    if (constraints.maxWidth > 600) return WideLayout();
    return NarrowLayout();
  },
)
```

---

## 5. Design & Theming

| Page | URL |
|---|---|
| Share styles with themes | https://docs.flutter.dev/cookbook/design/themes |
| Material design overview | https://docs.flutter.dev/ui/design/material |
| Migrate to Material 3 | https://docs.flutter.dev/release/breaking-changes/material-3-migration |
| Fonts & typography | https://docs.flutter.dev/ui/design/text/typography |
| Use a custom font | https://docs.flutter.dev/cookbook/design/fonts |
| Export fonts from package | https://docs.flutter.dev/cookbook/design/package-fonts |
| Fragment shaders | https://docs.flutter.dev/ui/design/graphics/fragment-shaders |
| Google Fonts package | https://pub.dev/packages/google_fonts |

### ThemeData pattern
```dart
MaterialApp(
  theme: ThemeData(
    useMaterial3: true,
    colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
    textTheme: TextTheme(bodyLarge: TextStyle(fontSize: 16)),
  ),
)

// Access in widget:
Theme.of(context).colorScheme.primary
Theme.of(context).textTheme.bodyLarge
```

### Custom theme extensions
```dart
// Define extension
class AppColors extends ThemeExtension<AppColors> {
  final Color brand;
  const AppColors({required this.brand});
  @override AppColors copyWith({Color? brand}) => AppColors(brand: brand ?? this.brand);
  @override AppColors lerp(AppColors? other, double t) => AppColors(brand: Color.lerp(brand, other?.brand, t)!);
}
// Register & access:
// theme: ThemeData(extensions: [AppColors(brand: Colors.teal)])
// Theme.of(context).extension<AppColors>()!.brand
```

### Typography (M3)
M3 type scale: `displayLarge/Medium/Small`, `headlineLarge/Medium/Small`, `titleLarge/Medium/Small`, `bodyLarge/Medium/Small`, `labelLarge/Medium/Small`.

---

## 6. Interactivity

### Add interactivity
**URL:** https://docs.flutter.dev/ui/interactivity

### Gestures

| Page | URL |
|---|---|
| Gestures introduction | https://docs.flutter.dev/ui/interactivity/gestures |
| Handle taps | https://docs.flutter.dev/cookbook/gestures/handling-taps |
| Drag outside app | https://docs.flutter.dev/ui/interactivity/gestures/drag-outside |
| Drag widget within app | https://docs.flutter.dev/cookbook/effects/drag-a-widget |
| Material touch ripples | https://docs.flutter.dev/cookbook/gestures/ripples |
| Swipe to dismiss | https://docs.flutter.dev/cookbook/gestures/dismissible |

**GestureDetector callbacks:** `onTap`, `onDoubleTap`, `onLongPress`, `onPanUpdate`, `onScaleUpdate`, `onHorizontalDragEnd`, `onVerticalDragEnd`.

**Gesture arena:** Flutter resolves conflicting gestures via arena. Use `GestureDetector(behavior: HitTestBehavior.opaque)` to claim all touches.

### Input & Forms

| Page | URL |
|---|---|
| Create and style a text field | https://docs.flutter.dev/cookbook/forms/text-input |
| Retrieve text field value | https://docs.flutter.dev/cookbook/forms/retrieve-input |
| Handle text field changes | https://docs.flutter.dev/cookbook/forms/text-field-changes |
| Manage focus in text fields | https://docs.flutter.dev/cookbook/forms/focus |
| Build a form with validation | https://docs.flutter.dev/cookbook/forms/validation |

```dart
// Form + validation pattern
final _formKey = GlobalKey<FormState>();
Form(
  key: _formKey,
  child: Column(children: [
    TextFormField(
      validator: (v) => v!.isEmpty ? 'Required' : null,
    ),
    ElevatedButton(
      onPressed: () { if (_formKey.currentState!.validate()) { /* submit */ } },
      child: Text('Submit'),
    ),
  ]),
)
```

### Other interactivity

| Page | URL |
|---|---|
| Display a snackbar | https://docs.flutter.dev/cookbook/design/snackbars |
| Actions & shortcuts | https://docs.flutter.dev/ui/interactivity/actions-and-shortcuts |
| Manage keyboard focus | https://docs.flutter.dev/ui/interactivity/focus |

---

## 7. Assets & Media

| Page | URL |
|---|---|
| Add assets and images | https://docs.flutter.dev/ui/assets/assets-and-images |
| Display images from internet | https://docs.flutter.dev/cookbook/images/network-image |
| Fade in images with placeholder | https://docs.flutter.dev/cookbook/images/fading-in-images |
| Play and pause video | https://docs.flutter.dev/cookbook/plugins/play-video |
| Transform assets at build time | https://docs.flutter.dev/ui/assets/asset-transformation |

### Asset declaration (pubspec.yaml)
```yaml
flutter:
  assets:
    - assets/images/
    - assets/icons/logo.svg
  fonts:
    - family: Roboto
      fonts:
        - asset: fonts/Roboto-Regular.ttf
        - asset: fonts/Roboto-Bold.ttf
          weight: 700
```

### Image loading
- `Image.asset('assets/img.png')` — bundled asset
- `Image.network('https://...')` — remote, cached in memory
- `Image.file(File path)` — local filesystem
- `FadeInImage` — shows placeholder while network image loads
- `cached_network_image` package — persistent disk cache

---

## 8. Navigation & Routing

| Page | URL |
|---|---|
| Navigation overview | https://docs.flutter.dev/ui/navigation |
| Add tabs | https://docs.flutter.dev/cookbook/design/tabs |
| Navigate to new screen | https://docs.flutter.dev/cookbook/navigation/navigation-basics |
| Send data to new screen | https://docs.flutter.dev/cookbook/navigation/passing-data |
| Return data from screen | https://docs.flutter.dev/cookbook/navigation/returning-data |
| Add a drawer | https://docs.flutter.dev/cookbook/design/drawer |
| Cupertino sheet | https://docs.flutter.dev/cookbook/design/cupertino-sheets |
| Deep linking | https://docs.flutter.dev/ui/navigation/deep-linking |
| App links (Android) | https://docs.flutter.dev/cookbook/navigation/set-up-app-links |
| Universal links (iOS) | https://docs.flutter.dev/cookbook/navigation/set-up-universal-links |
| Web URL strategies | https://docs.flutter.dev/ui/navigation/url-strategies |

### Navigator 1.0 (imperative)
```dart
// Push
Navigator.push(context, MaterialPageRoute(builder: (_) => DetailScreen(id: id)));
// Pop with result
Navigator.pop(context, result);
// Named routes
Navigator.pushNamed(context, '/detail', arguments: {'id': 1});
```

### Navigator 2.0 / Router API (declarative)
Used by `go_router`, `auto_route`, `routemaster`. Recommended for web + deep linking.

**go_router** (official recommended package):
```dart
final router = GoRouter(routes: [
  GoRoute(path: '/', builder: (ctx, state) => HomeScreen()),
  GoRoute(path: '/detail/:id', builder: (ctx, state) => DetailScreen(id: state.pathParameters['id']!)),
]);
MaterialApp.router(routerConfig: router)
// Navigate:
context.go('/detail/42');
context.push('/detail/42'); // pushable (back button works)
```

---

## 9. Animations & Transitions

| Page | URL |
|---|---|
| Introduction | https://docs.flutter.dev/ui/animations |
| Tutorial | https://docs.flutter.dev/ui/animations/tutorial |
| Implicit animations | https://docs.flutter.dev/ui/animations/implicit-animations |
| Animate container properties | https://docs.flutter.dev/cookbook/animation/animated-container |
| Fade widget in/out | https://docs.flutter.dev/cookbook/animation/opacity-animation |
| Hero animations | https://docs.flutter.dev/ui/animations/hero-animations |
| Page route transition | https://docs.flutter.dev/cookbook/animation/page-route-animation |
| Physics simulation | https://docs.flutter.dev/cookbook/animation/physics-simulation |
| Staggered animations | https://docs.flutter.dev/ui/animations/staggered-animations |
| Staggered menu animation | https://docs.flutter.dev/cookbook/effects/staggered-menu-animation |
| Animation API overview | https://docs.flutter.dev/ui/animations/overview |

### Animation tiers (simplest → most control)
1. **Implicit** (`AnimatedContainer`, `AnimatedOpacity`, `AnimatedPositioned`, `AnimatedPadding`, `TweenAnimationBuilder`) — set target value, Flutter animates automatically
2. **Explicit** (`AnimationController` + `Tween` + `AnimatedBuilder`) — full control, requires `TickerProviderStateMixin`
3. **Hero** — shared element transition between routes; wrap both widgets in `Hero(tag: same_tag)`
4. **Custom painter + animation** — `CustomPaint` + `AnimationController` for fully custom

```dart
// Implicit — simplest
AnimatedContainer(
  duration: Duration(milliseconds: 300),
  width: _expanded ? 200 : 100,
  color: _expanded ? Colors.blue : Colors.red,
  curve: Curves.easeInOut,
)

// Explicit — full control
class _MyState extends State<MyWidget> with SingleTickerProviderStateMixin {
  late AnimationController _ctrl;
  late Animation<double> _anim;

  @override void initState() {
    super.initState();
    _ctrl = AnimationController(vsync: this, duration: Duration(seconds: 1));
    _anim = Tween(begin: 0.0, end: 1.0).animate(CurvedAnimation(parent: _ctrl, curve: Curves.easeIn));
    _ctrl.forward();
  }
  @override void dispose() { _ctrl.dispose(); super.dispose(); }

  @override Widget build(BuildContext context) =>
    AnimatedBuilder(animation: _anim, builder: (ctx, child) => Opacity(opacity: _anim.value, child: child), child: Text('Hi'));
}
```

---

## 10. Accessibility

| Page | URL |
|---|---|
| Introduction | https://docs.flutter.dev/ui/accessibility |
| UI design & styling | https://docs.flutter.dev/ui/accessibility/ui-design-and-styling |
| Assistive technologies | https://docs.flutter.dev/ui/accessibility/assistive-technologies |
| Accessibility testing | https://docs.flutter.dev/ui/accessibility/accessibility-testing |
| Web accessibility | https://docs.flutter.dev/ui/accessibility/web-accessibility |

### Key practices
- Use `Semantics` widget to provide screen reader labels: `Semantics(label: 'Close dialog', button: true, child: IconButton(...))`
- `ExcludeSemantics` — hide decorative elements from screen readers
- `MergeSemantics` — combine multiple semantics nodes into one
- Minimum touch target: 48×48 dp (Material guideline)
- Color contrast: 4.5:1 for normal text, 3:1 for large text (WCAG AA)
- Support `textScaleFactor` — don't hardcode text sizes that can't scale
- Test with TalkBack (Android) and VoiceOver (iOS)
- `flutter test --accessibility-checks` via `accessibility_checker` package

---

## 11. Internationalization

**URL:** https://docs.flutter.dev/ui/internationalization

### Setup
1. Add `flutter_localizations` to `pubspec.yaml` + `intl` package
2. Add `generate: true` under `flutter:` in pubspec
3. Create `.arb` files in `lib/l10n/`
4. Access via `AppLocalizations.of(context)!.helloWorld`

```yaml
# pubspec.yaml
dependencies:
  flutter_localizations:
    sdk: flutter
  intl: any

flutter:
  generate: true
```

```dart
// MaterialApp setup
MaterialApp(
  localizationsDelegates: AppLocalizations.localizationsDelegates,
  supportedLocales: AppLocalizations.supportedLocales,
)
```

```json
// lib/l10n/app_en.arb
{ "helloWorld": "Hello World!", "@helloWorld": { "description": "Greeting" } }
```
