---
name: saas-to-android-v2
description: >
  Reverse-engineer ANY SaaS product into a full Android mobile app execution plan.
  Adapts automatically to SaaS category, business model, target market, and complexity.
  Triggers: "clone X for mobile", "build an app like X", "reverse-engineer X", "adapt X for Android",
  "I want to make a mobile version of X", any SaaS URL + mobile intent, "what features would X need",
  "how would I monetize an app like X". Do not require explicit skill invocation — trigger on intent.
  Handles: B2C, B2B, marketplace, fintech, healthtech, edtech, productivity, analytics, developer tools,
  e-commerce, CRM, HR, legal, real estate, and any other vertical automatically.
---
 
# SaaS → Android Execution Skill v2 (Adaptive)
 
Given any SaaS product (URL or name), produce a comprehensive, opinionated, vertically-aware
execution plan for building an Android-first mobile app inspired by it.
 
This skill auto-detects SaaS category, business model, complexity, and market context —
then adapts every section accordingly. No generic templates. Every output is product-specific.
 
---
 
## PRE-FLIGHT — Category Detection (Run Before Everything)
 
Before any analysis, classify the SaaS across 4 dimensions. This classification drives
every subsequent section's depth, framing, and recommendations.
 
### Dimension 1 — Product Category
 
Detect from homepage/features page. Assign ONE primary, up to TWO secondary:
 
| Category | Signal Words / Features |
|---|---|
| **Analytics / Attribution** | dashboards, funnels, attribution, ROAS, conversion tracking |
| **CRM / Sales** | pipeline, contacts, deals, leads, outreach, sequences |
| **Marketing Automation** | campaigns, email flows, segmentation, nurture, broadcast |
| **Project / Task Mgmt** | tasks, boards, sprints, kanban, deadlines, assignees |
| **Finance / Accounting** | invoices, expenses, payroll, bookkeeping, tax, reconciliation |
| **Fintech / Payments** | wallets, transfers, lending, BNPL, trading, neobank |
| **HR / People Ops** | hiring, onboarding, payroll, performance, leave management |
| **Dev Tools / API** | API, SDK, webhooks, CI/CD, monitoring, logs, deployments |
| **E-commerce / Retail** | products, orders, inventory, storefront, checkout, shipping |
| **Marketplace** | buyers + sellers, listings, bookings, reviews, commissions |
| **Edtech / LMS** | courses, lessons, quizzes, certificates, cohorts, progress |
| **Healthtech** | appointments, records, prescriptions, vitals, teleconsult |
| **Legal / Compliance** | contracts, e-sign, compliance, audits, policies |
| **Real Estate / PropTech** | listings, leads, tours, leases, property mgmt |
| **Productivity / Notes** | docs, notes, wikis, knowledge base, templates |
| **Communication / Collab** | messaging, video calls, channels, threads, presence |
| **AI / Automation** | AI agents, workflow automation, no-code, LLM-powered |
| **Security / Identity** | SSO, MFA, access control, threat detection, audit logs |
| **Data / ETL** | pipelines, connectors, warehouses, transforms, sync |
| **Other** | Describe explicitly |
 
### Dimension 2 — Business Model
 
| Model | Indicators |
|---|---|
| **B2C Freemium** | Self-serve signup, free tier, individual users, card-optional trial |
| **B2B SaaS (SMB)** | Team seats, per-user pricing, CRM/ops focus, <$500/mo plans |
| **B2B SaaS (Enterprise)** | "Contact sales", SSO, compliance, custom contracts, >$500/mo |
| **Marketplace (2-sided)** | Supply + demand sides, commission or listing fees |
| **Usage-Based** | API calls, events, rows, credits, compute — metered billing |
| **Transactional** | Per-transaction fee, no subscription, volume-driven |
| **Hybrid** | Freemium + seat expansion, or subscription + usage overage |
 
### Dimension 3 — Mobile Suitability Score
 
Rate 1–5 based on product nature. This determines MVP scope ambition.
 
| Score | Profile | Implication |
|---|---|---|
| 5 | Core workflow IS mobile (field work, on-the-go, real-time) | Build full feature parity in MVP |
| 4 | Mobile companion adds major value (alerts, quick actions, dashboards) | MVP = 70% of web features |
| 3 | Mobile useful but secondary (occasional check-ins, approvals) | MVP = dashboard + action layer only |
| 2 | Mobile awkward (complex config, heavy data entry) | MVP = read-only + notifications |
| 1 | Mobile impractical (dev tools, complex editors, multi-monitor workflows) | Flag strongly; suggest PWA or widget instead |
 
### Dimension 4 — Competitive Mobile Landscape
 
- Search Play Store for direct competitors in same category
- Note: ratings, review count, last update, review sentiment gaps
- Flag if category is saturated (>10 rated apps) vs open (<3 rated apps)
---
 
## Step 0 — Research Protocol (Adaptive Crawl)
 
**Always fetch in this order. Stop when sufficient signal gathered.**
 
1. `web_fetch` homepage → extract: headline, value prop, feature names, target audience signals
2. `web_fetch` /pricing → extract: tier names, prices, feature gates, trial terms, seat model
3. `web_fetch` /features or /product → extract: full feature list, integration partners
4. If B2B/Enterprise: `web_fetch` /customers or /case-studies → extract: company size, industry, use cases
5. If marketplace: identify supply side + demand side separately
6. `web_search` "[product name] reviews" → scan G2/Capterra/Reddit for pain points, missing features, churn reasons
7. `web_search` "[product name] android app" → check Play Store presence
**Extract per category (in addition to basics):**
 
| Category | Extra Extraction Targets |
|---|---|
| Fintech | Regulatory mentions (RBI, SEC, FCA), KYC/AML flow, supported payment rails |
| Healthtech | HIPAA/DPDP mentions, EHR integrations, prescription/teleconsult features |
| Marketplace | Commission structure, supply acquisition strategy, trust/safety features |
| Dev Tools | API rate limits, SDK languages, webhook types, SLA mentions |
| E-commerce | SKU management complexity, shipping integrations, returns workflow |
| HR/Payroll | Jurisdiction support, compliance certifications, employee count targets |
| Edtech | Content types (video/text/live), cohort vs self-paced, certificate issuing |
 
**Do NOT skip Step 0. All downstream quality depends on real product data.**
 
---
 
## Step 1 — SaaS Breakdown
 
### 1A. Core Analysis
 
- **Value prop** — one sentence: "[Product] helps [persona] do [job] without [pain]"
- **Primary revenue driver** — the single feature/gate that converts free → paid
- **Secondary revenue drivers** — upsell levers post-conversion
- **Retention mechanism** — what makes switching costly (data, habit, network, integrations)
- **Mobile presence** — existing Android/iOS app? If yes: Play Store rating, review count, top 3 complaints from reviews
### 1B. User Personas (Category-Adaptive)
 
Generate 3–5 personas. Adapt role titles and urgency drivers to the detected category:
 
| Field | What to Include |
|---|---|
| Name/Role | Job title specific to this SaaS's category |
| Company context | Size, industry, tech maturity |
| Core job-to-be-done | The specific task they use the product for |
| Mobile urgency | Why they need this on mobile specifically |
| Willingness to pay | Monthly budget, in local currency (check user location) |
| Churn trigger | What would make them cancel |
 
**Persona count by business model:**
- B2C: 3–4 personas (end-user archetypes)
- B2B SMB: 3–4 (buyer vs end-user distinction)
- B2B Enterprise: 4–5 (champion, admin, end-user, IT buyer, exec sponsor)
- Marketplace: always include supply-side AND demand-side personas separately
### 1C. Competitive Gap Assessment
 
Flag if source SaaS already has a strong Android app (≥4.0 rating, ≥1K reviews, updated <6mo):
- **If yes** → explicitly state "HEAD-ON COMPETITION INADVISABLE". Pivot strategy to niche (geography, vertical, price, modality). All subsequent sections adapt to niche angle.
- **If no** → proceed normally. Note gap as key opportunity.
---
 
## Step 2 — Feature Extraction & Prioritization
 
### 2A. Feature Tier Table
 
| Tier | Label | Criteria |
|---|---|---|
| 1 | Core | Directly delivers value prop on mobile; MVP blocker if missing |
| 2 | Supporting | Increases retention/conversion; ship in first 60 days post-launch |
| 3 | Advanced | Power-user, B2B, or platform features; Phase 3+ |
 
List every significant feature from the SaaS. Assign tier. Be exhaustive — don't compress.
 
### 2B. MVP Feature Set
 
State explicitly: "MVP ships with Tier 1 features only:" followed by a numbered list.
Include estimated dev effort per feature: [S=1–3d, M=3–7d, L=1–2w, XL=2–4w]
 
### 2C. Mobile Translation Issues (Category-Aware)
 
Map web patterns → mobile alternatives per category:
 
**Universal translations:**
| Web Pattern | Mobile Alternative |
|---|---|
| Dense data tables | Card tiles + expandable rows |
| Side panel / drawer | Bottom sheet |
| Hover tooltips | Long-press info popup |
| Multi-column dashboards | Scrollable widget stack, pinnable KPI cards |
| Tab bar (top) | Bottom navigation (max 5 tabs) |
| Right-click context menu | Swipe actions (left/right) |
| Modal dialogs | Full-screen sheets or inline expansion |
| Bulk select + actions | Multi-select with floating action bar |
 
**Category-specific translations:**
| Category | Specific Issue | Mobile Solution |
|---|---|---|
| Analytics | Multi-chart dashboards | Top KPI strip + swipeable chart carousel |
| CRM | Contact detail pages (many fields) | Tabbed profile cards (Activity / Info / Deals) |
| Project Mgmt | Kanban boards (wide) | Vertical swimlane list + horizontal scroll option |
| Finance | Spreadsheet-like ledger views | Transaction feed with filter chips |
| Marketplace | Map + list split view | Toggle map/list with FAB |
| Edtech | Video player + notes side-by-side | PiP video + bottom notes drawer |
| Healthtech | Long intake forms | Step wizard with progress indicator |
| Dev Tools | Code editors / terminals | Read-only log viewer + copy; defer editing to web |
| HR | Multi-step approval workflows | Single-action approval cards in notification tray |
 
Flag features that are **not viable on mobile** and recommend deferring to web or PWA.
 
---
 
## Step 3 — Mobile-First UX Adaptation
 
### 3A. Navigation Architecture
 
Define bottom nav tabs (max 5). Derive from product's primary user jobs, not its web sitemap.
 
```
Template: [Primary Feed/Home] [Core Action] [AI/Insights] [Notifications/Alerts] [Account]
Adapt tab names and order to the specific product's core workflows.
```
 
State: what goes in each tab, what's accessible via FAB, what's in overflow menu.
 
### 3B. Key Workflow Redesigns
 
For each of the top 3–4 user jobs identified in personas, map the complete mobile workflow:
- Trigger (push notification / habit / task reminder)
- Entry point (which tab / deep link)
- Steps (max 3 taps to complete core action — enforce this)
- Exit / confirmation pattern
### 3C. Engagement & Retention Patterns
 
Select patterns relevant to this SaaS's category:
 
| Pattern | Best For | Implementation |
|---|---|---|
| Daily digest push | Analytics, CRM, Finance | 8 AM summary of key metric vs yesterday |
| Action-required alerts | HR, Finance, Marketplace, Legal | Deep-link into specific record needing action |
| Streak / habit loop | Edtech, Productivity, Health | Visual streak counter + break-prevention nudge |
| Progress indicators | Edtech, Onboarding | % complete bars, milestone unlocks |
| Social proof feed | Marketplace, Community | "X users achieved Y today" |
| AI insight cards | Analytics, CRM, Finance | Proactive anomaly / recommendation surface |
| Approval shortcuts | HR, Finance, Legal | 1-tap approve/reject from notification |
 
### 3D. Market-Specific UX (Location-Aware)
 
**Always check user location from system prompt. Apply relevant block:**
 
**India / South Asia:**
- WhatsApp share on any report, invoice, or summary (critical for B2B workflows)
- UPI payment integration for in-app transactions
- Hindi + regional language toggle (at minimum Hindi)
- Data-lite mode: text-first rendering, lazy-load charts, <15MB APK target
- Low-end device optimization: test on Android Go spec (2GB RAM, 32GB storage)
- Truecaller SDK for phone-number-based auth (high conversion in India)
**Southeast Asia:**
- LINE / Zalo share integration (market-specific)
- GoPay / GrabPay / OVO payment options
- Bahasa Indonesia / Thai / Vietnamese locale support
**Africa / MENA:**
- M-Pesa / Flutterwave payment integration
- Offline-first architecture (unreliable connectivity)
- Arabic RTL layout support if MENA
**Latin America:**
- PIX / MercadoPago integration
- Portuguese (BR) / Spanish locales
- WhatsApp business API for notifications
**US / EU / Global:**
- Standard Material You design language
- Google Pay / Stripe
- GDPR consent flow if EU users in scope
---
 
## Step 4 — Technical Architecture
 
### 4A. Stack Selection (Adaptive)
 
**Base stack (always include):**
 
| Layer | Default Choice | Override Condition | Alternative |
|---|---|---|---|
| Android frontend | Kotlin + Jetpack Compose | Existing React Native team | React Native + Expo |
| State mgmt | ViewModel + StateFlow | Complex offline state | MVI + Room |
| Backend | FastAPI (Python) | No AI, simple CRUD | Node.js + Express |
| Database | PostgreSQL | Document-heavy data | MongoDB |
| Cache | Redis | — | — |
| Auth | Firebase Auth | Enterprise SSO needed | Supabase + SAML |
| Payments | RevenueCat + Google Play Billing | Transactional (not subscription) | Stripe + Razorpay |
| File storage | AWS S3 | Firebase-only stack | Firebase Storage |
| Push notifications | Firebase Cloud Messaging | — | — |
| Analytics | Firebase + Mixpanel | Privacy-first requirement | PostHog (self-hosted) |
| Crash reporting | Firebase Crashlytics | — | — |
 
**Category-specific additions:**
 
| Category | Additional Layer | Tool |
|---|---|---|
| Analytics / Attribution | Ad platform APIs | Meta Marketing API, Google Ads API, TikTok Ads API |
| Fintech | Payment rails | Razorpay / Stripe / Plaid |
| Fintech | KYC / AML | Onfido / DigiLocker API (India) |
| Healthtech | EHR integration | FHIR API / Practo API |
| Healthtech | Video consult | Daily.co / Agora |
| Marketplace | Maps | Google Maps SDK + Places API |
| Marketplace | Real-time | Supabase Realtime / Ably |
| Edtech | Video delivery | Mux / Cloudflare Stream |
| Edtech | DRM | Widevine L1 |
| CRM | Email sync | Gmail API / Nylas |
| Dev Tools | Webhooks / events | Hookdeck / Svix |
| HR | Payroll APIs | Razorpay Payroll / Darwinbox API |
| Real Estate | Property data | MagicBricks API / 99acres API (India) |
| AI-heavy | LLM | Anthropic Claude API (claude-sonnet) |
| AI-heavy | Streaming | SSE / WebSockets for token streaming |
| Offline-critical | Local DB | Room + WorkManager sync |
 
### 4B. Architecture Diagram
 
Generate ASCII block diagram tailored to detected stack. Always show:
- Android app layers (UI → ViewModel → Repository → Network/Local)
- Backend API service(s)
- Key external services (auth, DB, AI, platform APIs)
- Data flow direction arrows
Template (adapt service names):
```
┌──────────────────────────────────────┐
│           Android App                 │
│  ┌─────────┐  ┌─────────────────┐    │
│  │   UI    │  │   ViewModel /   │    │
│  │ Compose │←→│   StateFlow     │    │
│  └─────────┘  └────────┬────────┘    │
│                         │            │
│               ┌─────────▼────────┐   │
│               │   Repository     │   │
│               │ (Network + Room) │   │
│               └─────────┬────────┘   │
└─────────────────────────┼────────────┘
                           │ HTTPS/REST or GraphQL
                           ▼
┌──────────────────────────────────────┐
│           [Backend Service]           │
│   /auth  /[domain]  /ai  /webhooks   │
└───┬──────────┬──────────┬────────────┘
    │          │          │
    ▼          ▼          ▼
┌───────┐  ┌──────┐  ┌──────────────┐
│  DB   │  │Cache │  │ [AI / 3rd    │
│(PG/   │  │Redis │  │  Party APIs] │
│Mongo) │  └──────┘  └──────────────┘
└───────┘
    │
    ▼
┌─────────────────────────┐
│  [Domain-Specific APIs] │
│  [e.g. Meta / Razorpay] │
└─────────────────────────┘
```
 
### 4C. Non-Functional Requirements
 
State explicitly for this product:
- **Offline support needed?** (field workers, unreliable connectivity → yes → Room + WorkManager)
- **Real-time updates needed?** (trading, chat, live tracking → yes → WebSockets / SSE)
- **Data sensitivity?** (health/finance → encryption at rest + in transit mandatory)
- **Compliance requirements?** (see Step 6)
- **Target APK size** (India/emerging market → <20MB; global → <50MB acceptable)
---
 
## Step 5 — Monetization Strategy
 
### 5A. Model Selection (Business Model Adaptive)
 
**B2C Freemium:**
- 3-tier table: Free / Pro Monthly / Pro Yearly (or Lifetime)
- Gate: usage limits, feature locks, or storage caps
- Conversion trigger: user hits limit during high-intent moment
**B2B SMB:**
- 3-tier: Starter / Growth / Business (per seat or per workspace)
- Gate: seat count, integration depth, admin controls
- Trial: 14-day full access, no credit card
**B2B Enterprise:**
- Mobile app = companion to web; monetize via seat expansion
- Add-on: mobile seat surcharge or bundle
- No in-app purchase — invoice/PO flow
**Marketplace:**
- Demand side: free to browse, fee to transact (% commission or flat)
- Supply side: free listing → paid featured placement or subscription
- Liquidity phase: subsidize one side early
**Usage-Based:**
- Free credits to start (e.g., 100 events/mo)
- Pay-as-you-go above threshold
- No hard paywall — soft nudge at 80% usage
### 5B. Pricing Table
 
Always price in user's local currency (check system prompt location). Reference proven price anchors:
 
**India benchmarks:**
- Micro/freelancer tier: ₹299–₹499/mo
- SMB Pro: ₹999–₹2,499/mo
- Agency/Team: ₹3,999–₹7,999/mo
- Annual discount: 30–40% (frame as "2–4 months free")
**Global benchmarks:**
- Individual: $9–$19/mo
- Pro/Team: $29–$79/mo
- Business: $99–$299/mo
| Tier | Price | Included | Gate |
|---|---|---|---|
| Free | ₹0 / $0 | [List 3–5 features] | [Hard limit that creates pain] |
| Pro | [local price]/mo | [List 8–10 features] | [Expansion feature] |
| [Team/Agency/Business] | [local price]/mo | [Full feature set] | [Enterprise features] |
 
### 5C. Freemium Gate Strategy
 
- **What's free:** enough to demonstrate value, not enough to be complete
- **Paywall moment:** triggered at the highest-intent user action (not at signup)
- **Gate type:** choose ONE per product
  - Usage cap (X events, X records, X AI queries)
  - Feature lock (core works free, power features locked)
  - Seat cap (1 user free, team needs Pro)
  - History cap (last 7 days free, full history = Pro)
### 5D. Conversion & Retention Mechanics
 
- Trial: 14-day full Pro, no card (standard) OR 7-day for high-ARPU products
- FOMO trigger: "You hit X limit 3 times this week — upgrade to remove"
- Lock-in: identify which Pro feature makes data export painful (accrued data, custom configs, integrations)
- Annual nudge: show annual savings in ₹ on upgrade screen, not just %
---
 
## Step 6 — Legal & Ethical Considerations
 
### 6A. What You CAN Build
 
- Any concept, workflow, or user problem that the SaaS addresses (ideas are not IP-protected)
- Integrations with public APIs (Meta, Google, Stripe, etc.)
- Similar feature sets with original implementation
- Apps targeting same user persona from different angle
### 6B. What You CANNOT Do
 
- Copy brand, logo, name, tagline, or marketing copy
- Reproduce UI layouts (screenshot-for-screenshot)
- Scrape or reverse-engineer proprietary algorithms, ML models, prompt templates
- Use their registered trademarks in ASO (app title, keywords, description)
- Misrepresent affiliation or certification with source SaaS
### 6C. Vertical-Specific Legal Requirements
 
**Always include relevant block based on detected category:**
 
| Category | Requirements |
|---|---|
| **Fintech (India)** | RBI PA/PG license for payment processing; NBFC license for lending; DPDP Act compliance; PCI-DSS for card data |
| **Fintech (Global)** | PSD2 (EU), FinCEN registration (US), FCA authorization (UK) |
| **Healthtech (India)** | DPDP Act, NMC telemedicine guidelines, ABDM integration recommended |
| **Healthtech (Global)** | HIPAA Business Associate Agreement (US), GDPR Article 9 (EU special category) |
| **Marketplace** | Consumer protection act compliance, GST registration (India), clear T&C on liability |
| **HR / Payroll** | Labor law compliance per jurisdiction, employee data retention policies |
| **Legal / E-sign** | IT Act 2000 (India), eIDAS (EU), ESIGN Act (US) — digital signature validity |
| **Edtech (children)** | COPPA (US), DPDP consent for minors (India) — if any users under 18 |
| **All apps** | Play Store policies: no deceptive subscriptions, clear cancellation, privacy policy mandatory |
 
### 6D. Differentiation Mandate
 
Specify 2–3 concrete angles creating genuine novelty vs source SaaS:
 
**Axes to choose from:**
- Geography / language (localized for market source SaaS ignores)
- Price point (dramatically cheaper for SMB/emerging market)
- Vertical focus (source SaaS is horizontal; you go deep in one industry)
- Modality (source is web-only; you are mobile-native with offline)
- Integration depth (native integration with dominant local tools)
- AI-native (source has no AI layer; you build AI-first)
- Simplification (source is complex; you strip to 3 core jobs)
---
 
## Step 7 — Go-to-Market Plan
 
### 7A. Play Store ASO
 
**Title formula:** `[Core Job] [Category Noun] — [1 Differentiator]`
Examples:
- "Ad Attribution Tracker — AI ROAS Analytics"
- "Invoice & GST Billing — Small Business"
- "Hiring Pipeline CRM — Recruit Faster"
**Keyword strategy:**
- Short description (80 chars): lead with primary job-to-be-done keyword
- Long description: include 5–8 high-volume keywords naturally in first 167 chars (indexed by Play Store)
- Keywords to target: [product category] + [primary action verb] + [outcome] + [competitor brand names — check Play Store policy before using]
**Screenshot strategy (8 slots):**
1. Hero: core value prop headline over main dashboard
2. Key feature 1 (highest-intent feature)
3. Key feature 2
4. AI / automation feature (if applicable)
5. Notification / alert pattern
6. Social proof / result metric
7. Integration logos (trust signal)
8. Pricing / "Try free" CTA
**Rating prompt timing:** trigger after user completes 3rd meaningful action (not at install, not at first session). For B2B: after first successful workflow completion.
 
### 7B. Acquisition Channels (Category-Specific)
 
Select 4–6 channels most relevant to detected category + user location:
 
| Channel Type | B2C | B2B | Marketplace | India-specific |
|---|---|---|---|---|
| Communities | Reddit (r/[niche]), Discord | LinkedIn groups, Slack communities | Both sides separately | Facebook groups (niche), WhatsApp communities |
| Content | SEO blog, YouTube tutorials | LinkedIn thought leadership, case studies | Creator/seller guides | YouTube Hindi tutorials, regional blogs |
| Influencers | Micro-influencers in niche | Industry analysts, consultants | Top creators/sellers | Indian YouTube creators in vertical |
| Partnerships | Complementary app integrations | Agency partner program | Anchor supply partners | Razorpay/Zoho ecosystem, IndiaMART |
| Paid | Meta/Google UAC | LinkedIn Ads | Performance + supply-side ads | Meta India (cheaper CPIs) |
| Direct | — | Cold outreach (LinkedIn + email) | Manual supply seeding | WhatsApp outreach for SMBs |
 
**Early traction playbook (first 100 users):**
1. Post in 3 niche communities with problem-framing post (not product pitch)
2. DM 20 power users of source SaaS offering free Pro tier for feedback
3. Submit to Product Hunt (prepare 2 weeks ahead)
4. Leverage personal network for first 10 installs + reviews
### 7C. Feedback Loop
 
- **In-app feedback:** prompt at day 7 (post-onboarding, pre-churn window)
- **NPS survey:** day 30 (after first billing cycle)
- **Feature request:** upvote board (Canny or Featurebase) linked from settings
- **Community:** Discord or Skool community for beta users — invite at onboarding
- **Churn survey:** on cancellation, 3-option exit survey (price / missing feature / switching to X)
---
 
## Step 8 — Execution Roadmap
 
### 8A. Complexity Scoring
 
Before setting durations, score SaaS complexity:
 
| Factor | +1 point each |
|---|---|
| Has real-time features (chat, live data) | |
| Requires third-party API integrations (>3) | |
| Has AI / ML layer | |
| Marketplace (2-sided) | |
| Regulated industry (fintech, health, legal) | |
| Offline-first requirement | |
| Multi-language / multi-currency | |
| B2B with enterprise requirements | |
 
**Total score → MVP duration:**
- 0–2: MVP = 5–6 weeks
- 3–4: MVP = 7–9 weeks
- 5–6: MVP = 10–12 weeks
- 7+: MVP = 14+ weeks (consider phasing APIs out of v1)
### 8B. Phase Table
 
| Phase | Name | Duration | Key Deliverables |
|---|---|---|---|
| 0 | Validation | 2 wks | 15–20 customer discovery calls, demand signal, wireframes in Figma, API access applied |
| 1 | MVP Build | [score-derived] | Tier 1 features, auth, payment gate, core integrations |
| 2 | Polish + Launch | 3–4 wks | QA (real devices), ASO assets, Play Store submission, closed beta (50 users) |
| 3 | Growth | 6–8 wks | Tier 2 features, referral loop, retention mechanics, agency/partner channel |
| 4 | Scale | Ongoing | Localization, Tier 3 features, iOS port, enterprise tier, API/webhook offering |
 
**Phase 0 validation checklist:**
- [ ] 15 user interviews confirming mobile pain point
- [ ] Competitor Play Store analysis complete
- [ ] API access approved (Meta, Google, etc. if needed — can take 2–4 weeks)
- [ ] Tech stack finalized
- [ ] Figma wireframes for Tier 1 flows done
- [ ] RevenueCat + Firebase projects initialized
### 8C. Key Risks Table
 
Generate 5–7 risks specific to this SaaS's category and complexity score:
 
| Risk | Probability | Mitigation |
|---|---|---|
| [Category-specific risk 1] | H/M/L | [Specific mitigation] |
| [Category-specific risk 2] | H/M/L | [Specific mitigation] |
| API approval / rate limit delays | M | Apply Week 1; use mocked data for dev |
| Monetization conversion below 2% | M | A/B test paywall placement in Phase 2 |
| Source SaaS launches Android app during build | L–M | Lock in differentiation angle before launch; niche positioning |
| Play Store policy rejection | L | Review subscription policy before submission; use RevenueCat's compliant flow |
| [Regulatory/compliance risk if applicable] | H | [Jurisdiction-specific action] |
 
---
 
## Output Format Rules
 
- Use markdown tables for all structured data (features, stack, pricing, roadmap, risks)
- Use headers for all 8 steps + subsections
- ASCII diagram for architecture (no external image dependencies)
- **Always** derive step content from actual fetched product data — never use placeholder text in final output
- Be opinionated — specific recommendations, no hedging
- Compress prose: telegraphic, no filler, no "you might want to consider"
- Pricing always in user's local currency (check location in system prompt)
- End every output with:
  > "Want me to proceed with any specific deliverable — Kotlin scaffold, Figma wireframe spec, backend API schema, ASO copy, or Play Store listing?"
---
 
## Edge Cases
 
| Situation | Handling |
|---|---|
| **SaaS has highly-rated Android app (≥4.0, ≥1K reviews, updated <6mo)** | Flag in Step 1C. Shift ALL sections to niche/differentiation angle. Don't recommend head-on clone. |
| **B2B Enterprise only (no consumer tier)** | Personas = SMB owner + IT admin + end-user. MVP = mobile dashboard + approval actions only. Pricing = seat-based, no in-app purchase. |
| **Marketplace (2-sided)** | Treat supply + demand as separate apps or separate tab experiences. Monetization covers both sides. GTM covers supply seeding separately. |
| **API-gated SaaS (no public API)** | Flag in Step 4 as critical risk. Suggest: web scraping (ethical/legal check first), reverse-engineering (legal risk — flag), or building independent data layer. |
| **Hardware-dependent SaaS (IoT, POS, sensors)** | Flag mobile-only limitation. Suggest: companion app scope (device control / data view), Bluetooth/BLE integration if relevant. |
| **Regulated industry (fintech, health, legal)** | Step 6C compliance block is mandatory. Add compliance sprint to Phase 0. Add legal review milestone to Phase 1. |
| **No URL — just product name** | web_search first. If ambiguous (common name), ask for clarification before proceeding. |
| **User asks for one specific section** | Deliver that section only, in full depth. Don't force full 8-step output. |
| **SaaS is purely a dev tool / CLI** | Mobile Suitability Score = 1–2. Recommend companion app (dashboard + alerts only) or PWA. Flag that full mobile parity is inadvisable. |
| **SaaS is already shut down / acquired** | Note status. Proceed with analysis based on archived/fetched content — the market gap may still exist. |
| **User's location = India** | Apply all India-specific blocks: UPI, WhatsApp, Truecaller, vernacular, data-lite, ₹ pricing, DPDP compliance, low-end device optimization. |
