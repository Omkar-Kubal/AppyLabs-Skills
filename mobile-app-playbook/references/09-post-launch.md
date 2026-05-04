# Section 9: Post-Launch & Scaling

## 9.1 Analytics Integration

### Analytics Stack (recommended)
| Tool | Purpose |
|------|---------|
| Firebase Analytics | Core event tracking, funnels, audiences |
| Firebase Crashlytics | Crash / ANR reporting |
| Firebase Performance | App startup, network latency, screen rendering |
| Mixpanel (optional) | Advanced user behavior, cohort analysis |
| RevenueCat (if paid) | Subscription analytics, MRR, churn |

### Core Events to Track (Day 1)
```
app_open
user_signed_up           { method: "email" | "google" | "phone" }
user_logged_in
onboarding_completed     { steps_completed: int }
core_feature_used        { feature_name, context }
conversion_event         { plan, price, currency }
error_occurred           { error_code, screen, context }
notification_tapped      { campaign_id, screen }
share_triggered          { content_type, channel }
```

### Funnel to Monitor Weekly
```
Install → Open → Sign Up → Core Action → Return (Day 7)
         ↓
     Drop-off points = product improvement targets
```

### Key Metrics Dashboard
| Metric | Formula | Target (consumer app) |
|--------|---------|----------------------|
| DAU/MAU ratio | DAU ÷ MAU × 100 | > 20% |
| D1 Retention | Users active on Day 1 ÷ Installs | > 40% |
| D7 Retention | Users active on Day 7 ÷ Installs | > 20% |
| D30 Retention | Users active on Day 30 ÷ Installs | > 10% |
| Crash-free rate | 1 - (Crashed sessions ÷ Total) | > 99.5% |
| Avg session length | Total session time ÷ Sessions | App-specific |

---

## 9.2 User Feedback Loop

### Feedback Collection Channels
| Channel | When to Use |
|---------|------------|
| In-app rating prompt | After positive moment (task completed, milestone reached) |
| In-app feedback form | After negative moment or error |
| Play Store reviews | Monitor + respond within 24h |
| Support email | Complex issues |
| User interviews | Monthly, 3–5 users |
| NPS survey (in-app) | Quarterly, after 30 days of use |

### Review Response Templates
```
Positive (5-star):
"Thanks, [Name]! Really glad [feature] is working well for you.
We're working on [upcoming feature] — stay tuned!"

Negative (1–2 star):
"Hi [Name], sorry about this experience. 
Could you email us at support@app.com so we can fix this directly?
We take all feedback seriously and are looking into this."
```

### Smart Rating Prompt (Android)
```kotlin
// Use Google's In-App Review API (no custom dialog needed)
val reviewManager = ReviewManagerFactory.create(context)
reviewManager.requestReviewFlow().addOnCompleteListener { request ->
    if (request.isSuccessful) {
        reviewManager.launchReviewFlow(activity, request.result)
    }
}

// Trigger conditions:
// ✅ After user completes a key task (not on launch)
// ✅ After Day 3 of active use
// ❌ Never after a crash or error
// ❌ Never more than once per 30 days
```

---

## 9.3 Bug Tracking

### Bug Triage Process
```
Critical (P0)  — App crash / data loss / security issue
               → Fix within 24h → hotfix release
               
High (P1)      — Core feature broken for subset of users
               → Fix within 3 days → patch release

Medium (P2)    — Non-critical feature broken
               → Fix in next sprint

Low (P3)       — Visual / minor UX issues
               → Backlog
```

### Bug Report Template
```
Title: [Screen/Flow] — [What went wrong]
Priority: P0 / P1 / P2 / P3
Device: [Model, Android version]
Steps to reproduce:
  1.
  2.
  3.
Expected behavior:
Actual behavior:
Frequency: Always / Sometimes / Rarely
Crashlytics link: [if crash]
Screenshot / Recording: [attached]
```

---

## 9.4 Feature Iteration Roadmap

### Roadmap Planning Cadence
```
Weekly      → Bug fixes, minor UX improvements
Sprint (2w) → Incremental feature improvements
Monthly     → Feature releases (minor version bump)
Quarterly   → Major feature releases, architecture improvements
```

### Feature Prioritization Framework (RICE)
```
Score = (Reach × Impact × Confidence) ÷ Effort

Reach      = users affected per month
Impact     = 1 (minimal) to 3 (massive)
Confidence = 50% to 100%
Effort     = person-weeks

Higher score → higher priority
```

---

## 9.5 Performance Monitoring

### Ongoing Monitoring Checklist (weekly)
- [ ] Crashlytics: crash-free rate > 99.5%
- [ ] ANR rate < 0.47% (Google's threshold for "bad behavior" flag)
- [ ] Play Console: Android Vitals in "good" range
- [ ] Firebase Performance: cold start < 2s, slow frames < 25%
- [ ] Play Store rating: > 4.0 (respond to all 1-2 star reviews)

### Android Vitals — Thresholds
| Metric | Bad (triggers Play demotion) |
|--------|------------------------------|
| Crash rate | > 1.09% of daily sessions |
| ANR rate | > 0.47% of daily sessions |
| Excessive wakeups | > 10/hr while screen off |
| Stuck partial wake locks | > 1hr held continuously |

### Scaling Infrastructure Triggers
```
1k DAU      → Monitor, no changes needed (Firebase free tier)
10k DAU     → Optimize queries, enable caching, review Firebase costs
50k DAU     → Consider dedicated backend, CDN for assets
100k DAU    → Database read replicas, horizontal API scaling
500k+ DAU   → Microservices evaluation, dedicated infra team
```
