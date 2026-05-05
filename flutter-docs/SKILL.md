---
name: flutter-docs
description: >
  Comprehensive Flutter documentation reference covering all official docs.flutter.dev sections.
  Use this skill whenever the user asks about Flutter widgets, layouts, animations, state management,
  navigation, platform integration (Android/iOS/Web/Desktop), testing, performance, deployment,
  DevTools, app architecture, accessibility, internationalization, packages/plugins, or any Flutter
  concept. Trigger for questions like "how do I do X in Flutter", "what widget should I use for Y",
  "how to deploy Flutter to iOS", "Flutter state management options", "Flutter DevTools", "add Flutter
  to existing app", "Flutter performance", "Flutter testing", "Material 3 migration", or any Flutter
  development task — even if phrased casually. Always consult this skill before answering Flutter
  questions from memory, as it contains the full up-to-date doc structure with canonical URLs.
---

# Flutter Documentation Skill

Full reference for https://docs.flutter.dev — all three top-level sections.

## Quick routing — read the right reference file

| User asks about… | Load reference file |
|---|---|
| Widgets, layout, animations, gestures, navigation, theming, accessibility, i18n, assets, design | `references/ui.md` |
| State mgmt, networking, Firebase, architecture, platform channels, Android/iOS/Web/Desktop integration, packages, testing, performance, deployment, add-to-app | `references/beyond-ui.md` |
| DevTools, editors, SDK, pubspec, hot reload, Flutter concepts, architecture overview, constraints, migration guides, releases, cookbook | `references/resources.md` |

Always read the relevant reference file before answering. For cross-cutting questions (e.g. "build and test a Flutter app"), read multiple files.

---

## Flutter fundamentals (always-in-context)

### Core concept: Everything is a widget
Flutter UI = tree of widgets. Widgets are immutable descriptions of UI. Flutter re-renders changed subtrees efficiently via element/render tree reconciliation.

**Widget types:**
- `StatelessWidget` — no mutable state, rebuilds only when parent changes
- `StatefulWidget` — owns mutable state via `State` object, calls `setState()` to trigger rebuild
- `InheritedWidget` — propagates data down the tree efficiently (basis for Provider, Theme, MediaQuery)

### Constraint system (critical)
Flutter layout rule: **constraints go down, sizes go up, parent sets position.**
- Parent passes `BoxConstraints` (min/max width/height) to child
- Child picks its size within constraints
- Parent positions child
- Key insight: a widget cannot choose its own size freely — it must fit parent constraints
- Ref: https://docs.flutter.dev/ui/layout/constraints

### Build modes
- `debug` — assertions on, hot reload, DevTools, slower
- `profile` — like release but with profiling hooks; use for perf measurement
- `release` — optimized, no debug overhead
- Ref: https://docs.flutter.dev/testing/build-modes

### Hot reload vs hot restart
- **Hot reload** — injects updated code into running Dart VM; preserves state; ~instant
- **Hot restart** — full restart; resets state; slower
- Ref: https://docs.flutter.dev/tools/hot-reload

### Dart language essentials for Flutter
- Null safety enforced; use `?` for nullable, `!` to assert non-null, `??` for fallback
- `async`/`await` + `Future`/`Stream` for async work
- `const` constructors = compile-time constants = no rebuild
- Named parameters with `{}`, required keyword, default values
- Extension methods for adding functionality without subclassing

---

## Get started
| Page | URL |
|---|---|
| Quick start (install + first app) | https://docs.flutter.dev/install/quick |
| Custom setup (granular install) | https://docs.flutter.dev/install/custom |
| Learn Flutter (pathways) | https://docs.flutter.dev/learn |

---

## Reference links (external)
- **API docs (dart:* and package:flutter):** https://api.flutter.dev
- **pub.dev (packages):** https://pub.dev/flutter
- **DartPad (online IDE):** https://dartpad.dev
- **Flutter blog:** https://blog.flutter.dev
- **Dart docs:** https://dart.dev/guides

---

## How to use this skill

1. Identify which section(s) the question touches
2. Load the relevant `references/*.md` file(s)
3. Find the matching sub-section and URL
4. Provide answer with canonical doc link
5. For code examples: write idiomatic Dart/Flutter; prefer `const` constructors; follow null safety
6. For "which approach" questions: present official recommendations first, then community options
7. For migration questions: always check `references/resources.md` → Migration guides section
