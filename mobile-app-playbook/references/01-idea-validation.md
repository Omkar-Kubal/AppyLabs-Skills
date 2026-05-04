# Section 1: Idea & Validation

## 1.1 Problem Definition

**Framework: Problem → Pain → Proof**

```
Problem Statement Template:
"[Target user] struggles with [specific pain point] when [context/trigger].
 Current solutions fail because [gap]. Our app solves this by [approach]."
```

### Problem Scoring Matrix
| Criterion | Score (1–5) |
|-----------|-------------|
| Pain frequency (daily vs rare) | |
| Workaround difficulty | |
| Willingness to pay | |
| Market size (TAM) | |
| Technical feasibility | |
| **Total** | /25 |

> Threshold: Score ≥ 18 → proceed. Below 15 → redefine or pivot.

---

## 1.2 Target Audience

### User Persona Template
```
Name: [Persona name]
Age range: 
Occupation:
Device usage: [Primary Android device / Android version]
Tech literacy: [Low / Medium / High]
Key frustration:
Primary goal when using this app:
When they use it: [commute / work / home / all]
Competing apps they already use:
```

### Segmentation checklist
- [ ] Primary user (pays or drives core usage)
- [ ] Secondary user (benefits but doesn't pay)
- [ ] Decision maker (B2B: who approves purchase)
- [ ] Geographic focus locked in
- [ ] Android version floor set (recommend: API 26 / Android 8.0 minimum)

---

## 1.3 Market Research & Competitor Analysis

### Research Sources
| Source | What to extract |
|--------|----------------|
| Google Play Store (category) | Top apps, ratings, review themes |
| App Annie / data.ai | Download estimates, ranking trends |
| Reddit / Facebook Groups | Raw user complaints about competitors |
| G2 / Capterra (B2B) | Feature gaps, pricing data |
| Google Trends | Search interest over time |
| SensorTower | Revenue estimates, keyword data |

### Competitor Analysis Table
| App | Core Feature | Weakness | Rating | Downloads est. |
|-----|-------------|----------|--------|----------------|
| Competitor A | | | | |
| Competitor B | | | | |
| Competitor C | | | | |

### Positioning Quadrant
Plot competitors on axes:
- X-axis: Simple ←→ Feature-rich
- Y-axis: Free ←→ Premium

Identify white space → that's your positioning target.

---

## 1.4 MVP Feature Selection

### MoSCoW Framework
| Priority | Meaning | Criteria |
|----------|---------|----------|
| **Must Have** | MVP blockers | Core loop fails without it |
| **Should Have** | v1.1 | Enhances core but not required |
| **Could Have** | v2.0 | Nice-to-have, low effort |
| **Won't Have** | Backlog | Out of scope this cycle |

### MVP Litmus Test (per feature)
- [ ] Does removing it break the core user journey?
- [ ] Can it be faked/mocked in MVP?
- [ ] Does it require a separate backend system?
- [ ] Will users notice its absence immediately?

### Validation Methods (pre-build)
1. **Landing page test** → Build in 1 day, measure email signups
2. **Wizard of Oz** → Manual backend, real frontend
3. **Paper prototype** → 5 user walkthroughs, record confusion points
4. **Smoke test ad** → Run ₹500 Google/Meta ad, measure CTR on "Download now"
5. **Competitor review mining** → Scrape 1-star reviews → extract unmet needs

### Go / No-Go Criteria
```
Go if:
  - 3+ users describe the same pain unprompted
  - At least 1 persona willing to pay / use daily
  - No dominant competitor solves the problem adequately
  - Team has skills to build MVP in ≤ 8 weeks
```
