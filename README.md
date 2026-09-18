# Amtul Saboor — BI Portfolio

Four end-to-end business intelligence projects, each built twice from the same simulated dataset: once as an **interactive Tableau Public workbook** (parameters, calculated fields, auto-written commentary) and once as a **self-contained HTML dashboard** (hand-built SVG, no dependencies). The two versions reconcile to the same numbers.

| # | Project | Domain | Tableau Public | Web version |
|---|---------|--------|----------------|-------------|
| 1 | **Freight Spend WBR** — Transportation Scorecard | Supply chain / controllership | [Open workbook](https://public.tableau.com/app/profile/amtl/viz/FreightSpendWBR-TransportationScorecard/FreightSpendWBR) | [`dashboards/freight_scorecard.html`](dashboards/freight_scorecard.html) |
| 2 | **Reader Growth Pulse** — Subscription Analytics | Consumer subscription / growth | [Open workbook](https://public.tableau.com/app/profile/amtl/viz/ReaderGrowthPulse-SubscriptionAnalytics/ReaderGrowthPulse) | [`dashboards/growth_pulse.html`](dashboards/growth_pulse.html) |
| 3 | **Security Posture Scorecard** — Org and Team Risk | Information security / governance | [Open workbook](https://public.tableau.com/app/profile/amtl/viz/SecurityPostureScorecard-OrgandTeamRisk/SecurityPostureScorecard) | [`dashboards/security_scorecard.html`](dashboards/security_scorecard.html) |
| 4 | **Hospital Margin and Quality** — Health System Finance | Healthcare finance / quality | [Open workbook](https://public.tableau.com/app/profile/amtl/viz/HospitalMarginandQuality-HealthSystemFinance/HospitalMarginandQuality) | [`dashboards/hospital_margin.html`](dashboards/hospital_margin.html) |

Tableau Public profile: **[public.tableau.com/app/profile/amtl](https://public.tableau.com/app/profile/amtl)**

> All data is simulated with a fixed seed so every number is reproducible and no employer data is used.

---

## 1. Freight Spend WBR — Transportation Scorecard

[![Freight Spend WBR](https://public.tableau.com/static/images/Fr/FreightSpendWBR-TransportationScorecard/FreightSpendWBR/1.png)](https://public.tableau.com/app/profile/amtl/viz/FreightSpendWBR-TransportationScorecard/FreightSpendWBR)

**Question:** Is the transportation network hitting its cost, service and settlement targets this week, and what should leadership do about it?

**What it shows:** a nine-metric KPI scorecard for the selected week (cost per unit, variance to target, what-if cost per unit, spend, units, on-time %, damage rate, days to settle, network health score), a 26-week cost-per-unit trend against a target reference line, spend by mode, and a lane watchlist with the dollar opportunity per lane.

**Parameters (19):** targets for every KPI; amber/red RAG bands; minimum lane volume; four what-if inputs (diesel change, volume growth, rate change, intermodal shift) that reproject cost per unit; six KPI weights for the composite health score; selected week.

**Key calculations:** `Cost per Unit = SUM([Spend Usd]) / SUM([Units])` · what-if CPU applies fuel share × diesel change, rate change, intermodal savings and volume dilution · `Network Health Score` = weighted average of `MIN(1, target / actual)` attainment per KPI · commentary string branches on RAG status.

**Data:** `data/freight_lanes_weekly.csv` — 24 lanes × 3 regions × 4 modes × 6 carriers × 78 weeks (units, spend, on-time, damage, invoice accuracy, days to settle, fuel share).

---

## 2. Reader Growth Pulse — Subscription Analytics

[![Reader Growth Pulse](https://public.tableau.com/static/images/Re/ReaderGrowthPulse-SubscriptionAnalytics/ReaderGrowthPulse/1.png)](https://public.tableau.com/app/profile/amtl/viz/ReaderGrowthPulse-SubscriptionAnalytics/ReaderGrowthPulse)

**Question:** Where are new subscribers coming from, are they staying, and is the onboarding experiment worth shipping?

**What it shows:** KPI summary for a rolling window (gross adds, churn, net adds, new-subscriber revenue, monthly churn %, retention health), net adds by market × plan highlight table, channel-mix pie, weekly adds vs churn, an auto-written growth insight, and an A/B test verdict.

**Parameters:** window length (4–26 weeks); A/B control conversion %, variant conversion %, sample per arm. **Filters:** plan, channel, market (applied to all worksheets).

**Key calculations:** `Monthly Churn % = churn in window / (weeks ÷ 4.33) / ending active subscribers` · `Retention Health = MAX(0, MIN(100, 100 − 8 × churn))` · two-proportion z-test `z = (p2 − p1) / SQRT(p̄(1 − p̄)(2/n))` with a verdict at |z| ≥ 1.96 · growth insight compares the current window with the prior window of equal length (accelerating / slowing / steady at ±5%).

**Data:** `data/subscriptions_weekly.csv` — 52 weeks × 5 channels × 5 markets × 3 plans; `data/retention_curves.csv` — month-by-month retention by plan.

---

## 3. Security Posture Scorecard — Org and Team Risk

[![Security Posture Scorecard](https://public.tableau.com/static/images/Se/SecurityPostureScorecard-OrgandTeamRisk/SecurityPostureScorecard/1.png)](https://public.tableau.com/app/profile/amtl/viz/SecurityPostureScorecard-OrgandTeamRisk/SecurityPostureScorecard)

**Question:** How secure is each organisation and team, and where should leadership push first?

**What it shows:** composite score this quarter vs last, score by control domain, a team leaderboard coloured by grade, an exposure-vs-remediation bubble chart (open findings × median days to fix, sized by headcount, coloured by critical findings), and an auto-written insight.

**Parameters:** a weight for each of the six NIST-CSF-aligned domains (identity & access, patching, data protection, awareness training, incident response, third-party risk). Change a weight and the composite, every grade and the narrative re-score. **Filter:** org.

**Key calculations:** `Weighted Score = SUM([Score] × [Domain Weight] × [Headcount]) / SUM([Domain Weight] × [Headcount])` · `Grade` A ≥ 90 / B ≥ 80 / C ≥ 70 / D · `Critical Bucket` 0 / 1–2 / 3+.

**Data:** `data/security_team_domain.csv` — 24 teams × 5 orgs × 6 domains × 2 quarters (~2,900 people).

---

## 4. Hospital Margin and Quality — Health System Finance

[![Hospital Margin and Quality](https://public.tableau.com/static/images/Ho/HospitalMarginandQuality-HealthSystemFinance/HospitalMarginandQuality/1.png)](https://public.tableau.com/app/profile/amtl/viz/HospitalMarginandQuality-HealthSystemFinance/HospitalMarginandQuality)

**Question:** Where does the operating margin come from, and is quality keeping pace with finance?

**What it shows:** KPI tiles (net revenue, operating margin, days in A/R, readmission, HCAHPS), operating-margin trend by facility, service-line contribution sorted and coloured by margin, five revenue-weighted quality measures, and margin commentary with a payer-mix what-if.

**Parameters:** CMS-style benchmarks for readmission, HCAHPS top-box, infection index, mortality and average LOS; payer shift to commercial (points). **Filters:** facility, service line, quarter.

**Key calculations:** `Operating Margin % = (SUM([Net Revenue]) − SUM([Opex])) / SUM([Net Revenue])` · quality measures weighted by net revenue · `Quality Flags Met` counts measures inside benchmark (0–5) · `Payer Shift Value ($M) = gross charges × shift pts / 100 × (commercial collection rate − self-pay collection rate)`.

**Data:** `data/hospital_quarterly.csv` — 4 facilities × 6 service lines × 4 quarters with payer mix and quality measures.

---

## Repository layout

```
bi-portfolio/
├── README.md                      ← this file
├── dashboards/                    ← self-contained HTML versions (open in any browser)
│   ├── README.md                  ← design notes for the web versions
│   ├── freight_scorecard.html
│   ├── growth_pulse.html
│   ├── security_scorecard.html
│   └── hospital_margin.html
├── data/                          ← the CSVs behind every workbook and dashboard
├── tableau/BUILD_SPEC.md          ← parameters and calculated fields, for rebuilding in Tableau
└── docs/tableau_dashboards_guide.html  ← user guide: every visual, filter and parameter explained
```

## How the Tableau workbooks are built

Each workbook uses a single extract built from one CSV. Every KPI is a calculated field (no hard-coded numbers), every target or benchmark is a parameter, and the commentary panels are string calculations that branch with `IF / ELSEIF` on the live values, so they rewrite themselves whenever a filter or parameter changes. Filters are applied across all worksheets in a workbook so the dashboard stays consistent.

## Skills demonstrated

Tableau Public web authoring (parameters, LOD-free weighted aggregations, reference lines, highlight tables, manual sorts, dashboard actions) · KPI framework design and RAG reporting · what-if and sensitivity modelling · statistical testing (two-proportion z-test) · weighted composite scoring · synthetic data generation with reproducible seeds · hand-built SVG dashboards with an accessible colour system · plain-language documentation for business users.
