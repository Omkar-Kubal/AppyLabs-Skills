# Section 3: UI/UX Design

## 3.1 Design Principles

### Core Principles (non-negotiable)
| Principle | Rule |
|-----------|------|
| **Action-first** | Primary action visible without scroll on every screen |
| **One task per screen** | Each screen solves exactly one user goal |
| **Minimal chrome** | Nav, status bar, toolbars take ≤ 15% screen real estate |
| **Progressive disclosure** | Show only what's needed now; reveal complexity on demand |
| **Thumb-friendly** | Primary interactions within bottom 60% of screen |
| **Error prevention > error handling** | Disable invalid actions; validate inline |
| **Consistent feedback** | Every action has a response ≤ 100ms (visual) |

### Anti-patterns to Avoid
- [ ] Hamburger menu as primary navigation (use bottom nav instead)
- [ ] Modal dialogs for non-critical interruptions
- [ ] Full-screen loading spinners (use skeleton screens)
- [ ] Auto-playing media
- [ ] More than 5 items in bottom navigation
- [ ] Unlabeled icon-only navigation
- [ ] Forms longer than 5 fields without progress indicator

---

## 3.2 Wireframing & Prototyping

### Fidelity Progression
```
Lo-Fi (Day 1–2)
  Tool: Paper / Excalidraw / Balsamiq
  Goal: Validate flow, not visuals
  Share with: 3–5 real users

Mid-Fi (Day 3–5)
  Tool: Figma (wireframe kit)
  Goal: Validate information architecture
  Share with: Team + stakeholders

Hi-Fi (Week 2)
  Tool: Figma + Material 3 component library
  Goal: Pixel-accurate spec for devs
  Share with: Developers (handoff)
```

### Screens to Wireframe First (minimum)
1. Splash / onboarding (3–4 screens)
2. Login / Signup
3. Home / Dashboard
4. Primary feature screen
5. Detail / expanded view
6. Empty states (all major lists)
7. Error states (network, auth, not found)
8. Settings / Profile

### Figma Handoff Checklist
- [ ] All text styles named (H1–Body–Caption)
- [ ] All colors in styles panel
- [ ] All components use auto-layout
- [ ] Spacing uses 8dp grid
- [ ] All states designed (default, pressed, disabled, error)
- [ ] Export settings set for Android (1x, 2x, 3x)
- [ ] Dev mode enabled

---

## 3.3 Design System

### Color Palette (Material You / M3)
```
Primary:      Brand color — buttons, FAB, active states
On-Primary:   Text/icons on primary — usually white
Secondary:    Accent — chips, secondary actions
Surface:      Card backgrounds — light gray or white
Background:   Screen background
Error:        Red — destructive actions, validation
On-Surface:   Body text on surface — ~87% opacity black
Outline:      Borders, dividers
```

### Typography Scale (sp units)
| Style | Size | Weight | Use |
|-------|------|--------|-----|
| Display Large | 57sp | Regular | Hero headings |
| Headline L | 32sp | Regular | Page titles |
| Title L | 22sp | Medium | Section headers |
| Title M | 16sp | Medium | Card titles |
| Body L | 16sp | Regular | Primary body text |
| Body M | 14sp | Regular | Secondary body |
| Label L | 14sp | Medium | Buttons, tabs |
| Label S | 11sp | Medium | Captions, badges |

### Spacing System (8dp grid)
```
4dp  — tight internal padding (icon to label)
8dp  — small gaps (between list items)
16dp — standard screen margin
24dp — section spacing
32dp — large section breaks
48dp — hero/feature spacing
```

### Component Checklist
- [ ] Button (Primary, Secondary, Text, Icon)
- [ ] Input field (Default, Focus, Error, Disabled)
- [ ] Card (Elevated, Filled, Outlined)
- [ ] Bottom Navigation Bar
- [ ] Top App Bar
- [ ] Floating Action Button
- [ ] Chips (Filter, Input, Suggestion)
- [ ] Bottom Sheet (Modal, Persistent)
- [ ] Snackbar / Toast
- [ ] Dialog (Alert, Simple, Full-screen)
- [ ] Empty State
- [ ] Loading Skeleton

---

## 3.4 Mobile-First UX Patterns

### Navigation Patterns
| Pattern | When to Use |
|---------|-------------|
| Bottom Navigation | 2–5 top-level destinations |
| Navigation Drawer | 5+ destinations, secondary nav |
| Tabs | Switching views within a destination |
| Back Stack | Linear flows (checkout, onboarding) |

### Common UX Patterns
```
Pull-to-refresh     → List screens with remote data
Infinite scroll     → Feed-style content (not paginated)
Swipe to dismiss    → List items (with undo snackbar)
Long press          → Multi-select / contextual actions
Floating labels     → Form fields (shrinks on focus)
Bottom sheet        → Contextual options (not modals)
Collapsing toolbar  → Content-heavy detail screens
```

### Accessibility Baseline
- [ ] Minimum touch target: 48×48dp
- [ ] Color contrast ratio: ≥ 4.5:1 (text), ≥ 3:1 (UI elements)
- [ ] All interactive elements have contentDescription
- [ ] App usable with TalkBack enabled
- [ ] No information conveyed by color alone
- [ ] Font scaling tested to 200%
