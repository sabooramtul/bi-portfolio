# Amtul Saboor — BI portfolio dashboards

Four interactive dashboards, each a single self-contained HTML file (no build step, no external data calls — open it in any browser or host it on GitHub Pages). Every number on every page is recalculated in the browser from the filters and parameters, and the commentary panels are written by the dashboard itself from the current state, the way an automated WBR narrative would be.

All datasets are **simulated with a fixed seed** so the numbers are reproducible and defensible, and no employer data is used. Each file documents its data structure and calculation method in the footer.

---

## 1. Freight Spend WBR — `freight_scorecard.html`

**The question:** Is the transportation network hitting its cost, service and settlement targets this week, and what should leadership do about it?

**Mirrors:** weekly business review scorecards for transportation controllership (lane- and mode-level spend visibility, leadership KPI targets, RAG reporting).

**What it does**
- Six KPI tiles (cost per unit, total spend, units, on-time delivery, damage rate, days to settle) with 13-week sparklines and RAG status.
- **User parameters:** editable targets for every KPI; amber/red threshold bands; minimum lane volume; four what-if sliders (diesel price, volume growth, contract rates, intermodal shift) that reproject the selected week; and adjustable weights for a composite network health score.
- Scope selectors for region, mode, carrier and week; comparison toggle (WoW / YoY / vs target).
- 26-week cost-per-unit trend against target with the what-if projection overlaid; spend by mode; sortable lane watchlist with variance to target.
- Auto-generated commentary: headline status, spend movement, strongest and weakest KPI, largest lane opportunity in dollars, and the what-if outcome in words.

**Data:** 24 lanes across 3 regions, 4 modes, 6 carriers, 78 weeks; seasonal and fuel effects built in. Structure follows BTS Freight Analysis Framework lane/mode reporting.

---

## 2. Reader Growth Pulse — `growth_pulse.html`

**The question:** Where are new subscribers coming from, are they staying, and is the onboarding experiment worth shipping?

**Mirrors:** subscription growth analytics for a reading product — segmentation, cohort analysis, acquisition/engagement/retention metrics and A/B testing.

**What it does**
- Animated retention-health ring, active subscribers, gross/net adds, monthly churn, new-subscriber revenue.
- Filters for plan, acquisition channel, market and window (4 / 13 / 26 weeks).
- Channel-mix donut, weekly gross adds vs churn with a net-adds line, cohort retention heatmap (sequential single-hue scale), retention curves by plan, and an A/B test card that runs a two-proportion z-test and states a verdict.
- Insight panel rewrites itself on every filter: growth direction, largest and fastest-growing channel, highest-churn channel, plan-mix effect on month-6 retention, and a recommendation.

**Data:** 52-week subscription ledger, 5 channels × 5 markets × 3 plans; retention and conversion rates follow published SaaS/media benchmarks.

---

## 3. Security Posture Scorecard — `security_scorecard.html`

**The question:** How secure is each organization, team and job level, and where should leadership push first?

**Mirrors:** a company-wide security score dashboard creating visibility and accountability across orgs and levels.

**What it does**
- Composite score (0–100, graded A–D) from six control domains aligned to NIST CSF functions; **the user sets the weight of each domain** and everything re-scores.
- Filters for organization, job level and period (this vs last quarter).
- Radar of the selected scope against the company profile; exposure-vs-remediation bubble chart (open findings × median days to fix, sized by headcount, coloured by open critical findings); team leaderboard with level distribution and status.
- Narrative panel: grade and movement, weakest/strongest domain with the marginal effect of the heaviest weight, lowest team, slowest remediator, critical findings outstanding.

**Data:** 24 teams in 5 orgs, ~2,900 people, two quarters.

---

## 4. Hospital Margin & Quality — `hospital_margin.html`

**The question:** Where does the operating margin come from, and is quality keeping pace with finance?

**Mirrors:** financial analytics and quality-measure reporting for a multi-hospital system.

**What it does**
- KPI tiles: net patient revenue, operating margin, days in A/R, 30-day readmission, HCAHPS top-box — each against a benchmark.
- Filters for facility, service line and quarter.
- Gross-charges-to-operating-margin waterfall; payer-mix donut with collection rates; five quality measures as bullet charts against CMS benchmarks with target bands; service-line contribution margins with bar width proportional to revenue.
- Commentary: margin vs benchmark, the service line carrying the system and the one dragging it, the dollar value of a 1-point payer-mix shift, and the quality read.

**Data:** 4 facilities × 6 service lines × 5 payers × 4 quarters; benchmarks approximate CMS Hospital Compare national rates.

---

## Design and method notes

- Charts are hand-built SVG with a validated colour system: categorical hues assigned in a fixed CVD-safe order, one-hue sequential scales for magnitude, and status colours (good / watch / off track) kept separate from series colours and always paired with a label.
- Every chart has hover tooltips; every dashboard renders in light and dark themes and down to phone width.
- Fonts via Google Fonts; no other external dependencies.
