# Section 2: Product Planning

## 2.1 Feature Breakdown

### Core vs Advanced Matrix
| Feature | Category | Effort (S/M/L) | Impact (H/M/L) | Version |
|---------|----------|----------------|----------------|---------|
| | Core | | | MVP |
| | Core | | | MVP |
| | Advanced | | | v1.1 |
| | Advanced | | | v2.0 |

**Prioritization formula**: `Score = (Impact × 3) - (Effort × 2)`
- High/H = 3, Medium/M = 2, Low/L = 1

---

## 2.2 User Stories

### Template
```
As a [persona], I want to [action] so that [outcome].

Acceptance Criteria:
  - Given [context], when [trigger], then [result]
  - Given [context], when [edge case], then [fallback]
```

### Story Sizing (T-shirt)
| Size | Dev Days | Examples |
|------|----------|---------|
| XS | <0.5 | UI label change, config flag |
| S | 0.5–1 | Form field, simple list screen |
| M | 1–3 | CRUD screen, API integration |
| L | 3–7 | Auth flow, payment integration |
| XL | 7+ | Real-time system, complex sync |

---

## 2.3 App Flow Diagrams

### Core Flows to Map (minimum)
1. **Onboarding flow** → First launch → permission requests → account creation → home
2. **Core user journey** → Entry point → primary action → result/confirmation
3. **Auth flow** → Login / Signup / Password reset / Social login
4. **Error flow** → Network failure → empty states → retry logic
5. **Notification flow** → Trigger → permission → action → deep link destination

### Flow Notation (text-based)
```
[Splash] → [Onboarding?]
             ├── New user → [Sign Up] → [Verify Email] → [Profile Setup] → [Home]
             └── Returning → [Login] → [Home]
                              └── Forgot PW → [Reset Email] → [New PW] → [Home]

[Home]
  ├── [Feature A] → [Detail] → [Action] → [Confirmation]
  ├── [Feature B] → ...
  └── [Profile] → [Settings] → [Logout]
```

---

## 2.4 Tech Stack Selection

### Decision Framework

#### Frontend
| Option | Best For | Avoid When |
|--------|----------|-----------|
| Kotlin + Jetpack Compose | Native perf, deep Android integration | Team knows only JS |
| Flutter | Cross-platform (Android + iOS), fast UI iteration | Needs platform-specific plugins |
| React Native | Web team shifting to mobile, JS codebase | High-perf graphics, complex animations |

#### Backend
| Option | Best For | Avoid When |
|--------|----------|-----------|
| Firebase | MVP, solo dev, real-time, auth included | GDPR-strict regions, complex queries |
| Supabase | Open-source Firebase alt, SQL-first | Real-time at massive scale |
| Node.js + Express | Flexible REST, JS team | Strict type safety required |
| NestJS | Structured Node, TypeScript, enterprise | Small teams, over-engineering risk |
| Django / FastAPI | Python teams, ML integration | Mobile-only teams |

#### Database
| Layer | Option | Use Case |
|-------|--------|---------|
| Local | Room (SQLite) | Offline-first, structured data |
| Local | DataStore | Key-value prefs, simple state |
| Remote | Firestore | Flexible, real-time sync |
| Remote | PostgreSQL | Relational, complex queries |
| Remote | MongoDB | Flexible schema, fast iteration |
| Cache | Redis | Session, rate limiting, pub-sub |

#### Key APIs & Services
| Category | Recommended |
|----------|------------|
| Auth | Firebase Auth, Auth0, Clerk |
| Payments | Razorpay (India), Stripe, Google Pay API |
| Push notifications | FCM (Firebase Cloud Messaging) |
| Analytics | Firebase Analytics, Mixpanel |
| Crash reporting | Firebase Crashlytics, Sentry |
| Storage | Firebase Storage, AWS S3, Cloudflare R2 |
| Maps | Google Maps SDK, Mapbox |
| AI/ML | Google ML Kit (on-device), Gemini API |

### Stack Decision Checklist
- [ ] Team's strongest language locked in
- [ ] Offline requirement assessed → Room vs cloud-only
- [ ] Real-time requirement assessed → Firestore vs REST polling
- [ ] Budget for backend services estimated
- [ ] GDPR/data residency requirements checked
- [ ] Third-party SDK licenses reviewed
