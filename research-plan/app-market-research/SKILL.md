---
name: app-market-research
description: >
  Deep market research, competitive analysis, and product strategy for mobile apps.
  Use this skill whenever a user shares a Play Store or App Store URL and asks for market research,
  growth strategy, competitor analysis, monetization advice, product roadmap, or feature prioritization.
  Also trigger when a user describes their app and asks "how do I scale", "who are my competitors",
  "what features should I build", "how do I monetize", or "help me reach 1M users".
  The skill fetches live app store data, runs live web searches for competitors and market size,
  produces a structured multi-section strategic document, a visual roadmap, and a downloadable
  executive summary. Always use this skill — even for partial requests like "analyze my app market"
  or "what's the competition like for my app".
---
 
# App Market Research Skill
 
Deep, web-grounded competitive intelligence and product strategy for any mobile app.
Adapts to any category: productivity, fintech, gaming, social, dev tools, health, etc.
 
---
 
## Phase 0: App Profile Extraction
 
### Step 1: Fetch app store listing
If user provides a Play Store or App Store URL, use `web_fetch` to retrieve the full listing.
Extract:
- App name, category, developer name & location
- Core features (from description)
- Screenshots (infer UX quality from available info)
- Download count / rating / review count (proxy for traction)
- Last updated date
- In-app purchases / price (monetization signal)
- Related apps (competitor signal)
If user provides info manually (not a URL), work with what they give. Ask for missing critical fields only if essential (traction, monetization, target audience).
 
### Step 2: Confirm app profile before researching
Summarize extracted profile back to user in 3-5 sentences. Flag critical unknowns.
Ask one clarifying question if needed (e.g., "Is this B2C or B2B?" or "Any revenue currently?").
Don't proceed to deep research until you understand what the app actually does.
 
---
 
## Phase 1: Market Intelligence (Web Research)
 
Run these web searches in sequence. Adapt queries to the specific app category.
 
### 1A: Market Size & Growth
```
web_search: "[app category] market size 2025 global India
web_search: "[specific niche] mobile app market growth CAGR 2025
```
Extract: TAM (total addressable market), CAGR, regional breakdowns, key growth drivers.
Flag: India vs. global split (especially important for Indian developers).
 
### 1B: Key Trends
```
web_search: "[app category] trends 2025 2026 user behavior
web_search: "AI [app category] tools adoption 2025
```
Extract: 5 key trends shaping the niche. Map each trend to an opportunity or threat for the app.
 
### 1C: User Behavior
```
web_search: "[app category] user pain points complaints 2025
web_search: "what do users want [app category] app missing features
```
Extract: Core user jobs-to-be-done, recurring complaints, unmet needs.
 
---
 
## Phase 2: Competitor Research
 
### 2A: Find Top Competitors
```
web_search: "best [app category] apps 2025 [specific use case]
web_search: "[app name] alternatives competitors 2025
web_search: Play Store "top [category]" apps [niche keywords]
```
 
Identify 4-6 real competitors. For each, fetch their Play Store or App Store listing if possible.
 
### 2B: Competitor Deep Dive
For each competitor, research:
```
web_search: "[competitor name] pricing monetization strategy
web_search: "[competitor name] reviews complaints problems
web_search: "[competitor name] user base downloads growth
```
 
Build competitor matrix covering:
- Download range / DAU estimate
- Core strengths (what they do well)
- Core weaknesses (user complaints, limitations)
- Monetization model (freemium, subscription, ads, one-time, B2B)
- What they do BETTER than the user's app
- What gap they leave open
### 2C: Direct Threat Assessment
Check if any major platform (Google, Apple, Meta, Microsoft, or industry-specific incumbent) has released an **official or free alternative**. This is critical — a "900-lb gorilla" competitor changes the entire strategy.
 
---
 
## Phase 3: Gap Analysis
 
Synthesize Phase 1 + Phase 2 into:
 
**What users complain about (existing apps):**
Top 5 recurring complaints from reviews + forums. These are addressable opportunities.
 
**Untapped opportunities:**
Cross-reference user complaints with competitor weaknesses. What's consistently missing across ALL competitors? That's the gap.
 
**Where the app can win:**
Identify 2-3 specific, defensible positions. Be honest: not every app can win everywhere. Recommend niche leadership over head-to-head battles with dominant players.
 
---
 
## Phase 4: Feature Expansion Strategy
 
Generate 10 high-impact features, prioritized into 3 tiers.
 
### Tier 1: MUST-BUILD (Months 0-3) — Core differentiation
Features that directly address the identified gaps and make the app distinct from competitors.
Each must answer: "Why does this make users choose us over [top competitor]?"
 
### Tier 2: SHOULD-BUILD (Months 4-6) — Market expansion
Features that expand TAM, improve retention, or unlock new monetization.
Include at least one team/social/collaborative feature if applicable (often 5x TAM unlock).
 
### Tier 3: NICE-TO-BUILD (Month 7+) — Virality & platform
Features that create network effects, viral loops, or long-term moats.
 
For each feature include:
- Feature name
- Why it matters (user/business impact)
- Effort estimate (Low / Medium / High)
- ROI score (1-10)
- Build timeline (days)
- Success metric
---
 
## Phase 5: Growth Strategy
 
### Organic
- Community & grassroots: subreddits, Discord, dev communities relevant to niche
- Content marketing: blog, YouTube, dev.to, Medium (adapt to audience)
- Product-led growth: free tier design, onboarding optimization
- Partnership targets: complementary tools, platforms, newsletters
- Cost and impact estimates for each
### Paid Acquisition
- Best paid channels for this niche (Google UAC, Meta, Reddit Ads, niche newsletters)
- Budget ranges and expected CPA/ROAS
- Keyword targets for the niche
### Viral Loops & Network Effects
- Referral mechanics that fit the product
- Collaboration features that create network effects
- Sharing hooks (user-generated, performance metrics, etc.)
- Viral coefficient estimates
---
 
## Phase 6: App Store Optimization (ASO)
 
Analyze and recommend improvements for:
 
**Title:** Current → Optimized (include primary keyword + differentiator)
**Short description:** Lead with pain point solved, not feature list
**Keywords:** Top 5-10 search terms by volume + intent fit
**Screenshot sequence:** Recommend 6-7 screenshots showing the core value loop
**Rating strategy:** When + how to request reviews for maximum response rate
**Localization:** Which markets to localize for first, based on category data
 
---
 
## Phase 7: Monetization Plan
 
Assess best revenue models for this specific niche:
 
**Model comparison table:** Freemium / Subscription / One-time / Usage-based / B2B / Ads
- Pros/cons for this specific app
- Industry benchmarks (conversion rates, ARPU, churn)
**Recommended pricing tiers:**
- Free tier: what to include (enough to drive activation, not enough to kill conversion)
- Paid tier(s): price anchoring, feature gating, team expansion pricing
- Enterprise: if applicable (custom, seat-based, SLA)
**Upsell ideas:**
- In-app upgrade moments (based on feature usage patterns)
- Email sequences for free-to-paid conversion
- Team expansion playbook (if applicable)
---
 
## Phase 8: TAM/SAM/SOM Honest Assessment
 
This section must be honest, not aspirational.
 
Compute realistic market size for THIS specific app:
- TAM: Total global market for the category
- SAM: Subset of users who actually need this app's specific features
- SOM: Realistic market share achievable in 18 months given competition and resources
Flag if the target (e.g., "1M users") is achievable given the niche constraints.
If it's not, say so clearly and explain what would need to change (pivots, new segments, etc.).
 
---
 
## Phase 9: Product Roadmap (30 / 90 / 180 Days)
 
### 30-Day Sprint
- 3-5 must-have features (core differentiation)
- Launch readiness tasks (ProductHunt, blog, communities)
- Success metrics to confirm product-market fit
### 90-Day Sprint
- 3-5 growth features (expansion + retention)
- Marketing milestones
- Revenue targets
### 180-Day Sprint
- Scale features (team, enterprise, marketplace, API)
- Sales/partnership milestones
- ARR targets
Include a Month 3 GO/NO-GO decision point with explicit success criteria.
If the criteria are not met, what should the founder do? (Pivot options, niche down, acquisition approach, or shutdown)
 
---
 
## Phase 10: Output Format
 
### Visual Roadmap (always include)
Use `visualize:show_widget` to generate an SVG/HTML roadmap showing:
- Phase timeline (kanban-style phases)
- Feature priority matrix (must/should/nice)
- Metrics targets per phase
- Revenue milestones
### Written Analysis (full document)
Structure:
```
# [App Name]: Strategic Market Analysis
 
## I. App Profile
## II. Market Analysis (size, trends, user behavior)
## III. Competitor Breakdown (table + narrative)
## IV. Gap Analysis
## V. Feature Expansion Strategy (10 features, 3 tiers)
## VI. Growth Strategy
## VII. ASO
## VIII. Monetization Plan
## IX. TAM/SAM/SOM Honest Assessment
## X. Product Roadmap
## XI. Brutal Honesty: Core Problems + Success Path
```
 
Save as `.md` file to `/mnt/user-data/outputs/[appname]_strategy_analysis.md`
 
### Executive Summary (always include)
A separate, shorter file (800-1200 words) covering:
- Situation summary (2-3 sentences)
- Honest TAM table
- Strategic recommendation (Go/No-Go with rationale)
- 30-day action items (checklist format)
- Critical dependencies (what must be true for this to work)
- Decision point at Month 3 (explicit pass/fail criteria)
- Biggest risks rank-ordered
Save as `/mnt/user-data/outputs/EXECUTIVE_SUMMARY.md`
 
Use `present_files` to share both files with the user at the end.
 
---
 
## Tone & Approach Rules
 
**Be brutally honest.** If the market is too crowded, say so. If a competitor is free and official, say so. If 1M users is unrealistic, say so. Founders need truth, not flattery.
 
**Be specific.** "Build a kanban board for multi-session agent management" > "improve UX". Every recommendation needs a concrete action + reason.
 
**Calibrate to traction.** 50 downloads ≠ 500k downloads. Recommendations for a pre-PMF app (market discovery, differentiation, survival) differ from post-PMF (scaling, CAC, LTV optimization). Adjust accordingly.
 
**Identify existential threats first.** Before going deep on features, check if there's a dominant free/official alternative. If there is, restructure the whole strategy around it (avoid head-to-head, find gaps, build moats in areas they ignore).
 
**TAM honesty.** If the realistic SOM is 10k users, don't pretend it's 1M. Explain what pivot would be needed to reach the stated goal.
 
**Prioritize with conviction.** Don't list 20 equal features. Force-rank into MUST/SHOULD/NICE and explain why.
 
---
 
## Adaptive Calibration by App Stage
 
| Stage | Downloads | Focus |
|---|---|---|
| Pre-launch | 0 | Market validation, competitor positioning, MVP feature selection |
| Early traction | <1k | PMF signals, differentiation, ASO, organic growth |
| Growing | 1k-50k | Retention, monetization, CAC/LTV, team expansion |
| Scaling | 50k+ | Paid UA, platform features, international, enterprise |
| Plateau | Flat growth | Reposition, new segment, pivot, or double down on niche |
 
Identify the stage from traction data. Recommend stage-appropriate strategies only.
 
---
 
## Adaptive Calibration by App Category
 
Adjust research focus by category:
 
**Dev Tools / B2D:** Developer communities (HN, Reddit, ProductHunt, GitHub), CLI/terminal workflows, open-source alternatives, enterprise sales motion, self-hosted vs. cloud privacy concerns.
 
**Consumer / Social:** Viral coefficients, retention loops (D1/D7/D30), network effects, content moderation, creator economy, platform dependency risks.
 
**Fintech:** Regulatory constraints (SEBI, RBI for India), trust-building, compliance costs, competitor moats (banking licenses, partnerships), freemium conversion rates.
 
**Health / Wellness:** Clinical credibility, user habit formation (streak mechanics), medical disclaimers, data privacy (HIPAA, India DPDPA), referral programs.
 
**Productivity:** Workflow integration (not standalone), team features priority, enterprise sales cycle, usage-based pricing, integrations moat.
 
**Gaming:** ARPU benchmarks, IAP design, ad monetization, LiveOps calendar, genre-specific metrics (D1/D7/D30 benchmarks differ dramatically by genre).
 
---
 
## Research Quality Checklist
 
Before finalizing output, verify:
- [ ] At least 4 web searches run (market size, trends, competitors, user complaints)
- [ ] Competitor table has 4-6 real apps with real data (not generic)
- [ ] TAM/SAM/SOM numbers are sourced, not invented
- [ ] Feature list has 3 tiers with conviction (not 10 equal items)
- [ ] Monetization recommendation matches the category and stage
- [ ] Roadmap has explicit Month 3 GO/NO-GO decision point
- [ ] Executive summary has concrete 30-day action items
- [ ] Brutal honesty section addresses the biggest existential threat
- [ ] Visual roadmap created via `visualize:show_widget`
- [ ] Both files saved and presented via `present_files`