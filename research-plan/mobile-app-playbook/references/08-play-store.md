# Section 8: Google Play Store Launch

## 8.1 Play Console Setup

### Account Setup Checklist
- [ ] Google Play Console account created ($25 one-time fee)
- [ ] Developer profile completed (name, email, address)
- [ ] D-U-N-S number obtained (required for organizations)
- [ ] Payment profile linked (for paid apps / in-app purchases)
- [ ] Service account created for CI/CD API access
- [ ] Team members added with appropriate roles

### App Creation Checklist
- [ ] New app created in Console
- [ ] Default language set
- [ ] App category selected
- [ ] Free / Paid decision locked
- [ ] Content rating questionnaire completed
- [ ] Target audience / content settings configured
- [ ] Data safety form completed (critical — reflects privacy policy)
- [ ] App access instructions added (if account required for review)

---

## 8.2 App Store Optimization (ASO)

### ASO Elements Priority
| Element | Impact | Character Limit |
|---------|--------|----------------|
| App title | ⭐⭐⭐⭐⭐ | 50 |
| Short description | ⭐⭐⭐⭐ | 80 |
| Long description | ⭐⭐⭐ | 4000 |
| Keywords in title/desc | ⭐⭐⭐⭐⭐ | — |
| Screenshots | ⭐⭐⭐⭐⭐ | — |
| Feature graphic | ⭐⭐⭐ | 1024×500px |
| App icon | ⭐⭐⭐⭐⭐ | 512×512px |
| Ratings & reviews | ⭐⭐⭐⭐⭐ | — |

### Title Formula
```
[Brand Name]: [Primary Keyword] - [Secondary Benefit]
Example: "Fintrack: Budget & Expense Tracker"
```

### Short Description Formula (80 chars)
```
[Primary action verb] + [core value prop] + [differentiator]
Example: "Track expenses effortlessly. Auto-categorized, no setup needed."
```

### Long Description Structure
```
Paragraph 1 (hook, 2–3 lines): What problem does this solve?
Paragraph 2 (features): 5–7 key features as bullets
Paragraph 3 (social proof): Downloads, ratings, press mentions
Paragraph 4 (CTA): Download call to action
Paragraph 5 (keywords): Natural-language keyword integration
Footer: Links to privacy policy, support email
```

### Keyword Research Process
1. Seed keywords from competitor titles
2. Expand using: Play Store suggestions, AppFollow, Sensor Tower
3. Score by: search volume × (1 / difficulty)
4. Target 20–30 keywords total
5. Embed naturally — no keyword stuffing

---

## 8.3 Visual Assets

### Screenshots (Required)
| Screen size | Required | Recommended count |
|-------------|----------|------------------|
| Phone (16:9) | Yes | 4–8 |
| 7" tablet | Recommended | 4 |
| 10" tablet | Recommended | 4 |

### Screenshot Best Practices
- [ ] First screenshot = strongest value prop (shown in search results)
- [ ] Show app in action (real screens, not illustrations only)
- [ ] Add context overlays / captions on screenshots
- [ ] Consistent device frame across all screenshots
- [ ] Use lifestyle + UI combination for max impact
- [ ] Size: 320–3840px, ratio 16:9 or 9:16
- [ ] Tools: AppLaunchpad, Previewed, Figma device mockups

### App Icon Rules
- [ ] 512×512px PNG, no alpha on background
- [ ] Works on all background colors (test on white, black, gray)
- [ ] No text smaller than 12pt equivalent
- [ ] Recognizable at 48×48dp (small size)
- [ ] Matches launcher icon in app

---

## 8.4 Pricing & Monetization Strategy

### Monetization Models
| Model | Best For | Avg Revenue/DAU |
|-------|----------|----------------|
| Freemium | Consumer apps, large funnel | Low (2–5% convert) |
| Subscription | Productivity, tools, content | High LTV |
| One-time purchase | Utilities, niche tools | Medium |
| In-app purchases | Games, consumption apps | Variable |
| Ads (AdMob) | Free apps, high volume | Low ($0.5–3 CPM India) |
| B2B/Enterprise | Workplace tools | High, small volume |

### Subscription Pricing (India benchmarks)
```
Tier 1: ₹99–149/month or ₹499–799/year   (personal, basic)
Tier 2: ₹199–299/month or ₹999–1499/year (personal, pro)
Tier 3: ₹499–999/month                   (team/business)
Annual discount: ~40% vs monthly (incentivize annual)
```

---

## 8.5 Compliance

### Permissions Checklist
- [ ] Only request permissions needed for core features
- [ ] Request permissions at point of use (not on launch)
- [ ] Rationale shown before permission dialog (if non-obvious)
- [ ] Graceful degradation if permission denied
- [ ] No `READ_CALL_LOG`, `READ_SMS` unless core to app (triggers review)

### Policy Compliance Checklist
- [ ] Privacy policy URL live and accessible
- [ ] Privacy policy covers: data collected, sharing, deletion rights
- [ ] Terms of service live
- [ ] Data safety section in Play Console matches actual behavior
- [ ] No misleading screenshots or descriptions
- [ ] No competitor brand names misused
- [ ] Ads comply with Play Ads policy
- [ ] Financial / health apps: additional declarations completed
- [ ] Apps targeting kids: Families policy compliance verified

### Release Tracks Strategy
```
Internal testing (up to 100 users)
    → Share with team, fast turnaround, no review
    
Closed testing / Alpha (specific user list)
    → Early adopters, beta testers
    
Open testing / Beta (public opt-in)
    → Broad testing, appears on Play Store as "Early access"
    
Production (staged rollout recommended)
    → Start at 10% → monitor crash rate → expand to 100%
    → Staged rollout: halt if crash rate > 1% above baseline
```
