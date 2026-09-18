# Tableau Public build spec — Amtul Saboor portfolio

Four workbooks, one CSV each (plus small lookup tables). Parameters and calculated fields listed so each can be rebuilt in Tableau Public web authoring or Tableau Desktop.

## 1. Freight Spend WBR — `freight_lanes_weekly.csv` (+ `freight_kpi_targets.csv`)
Parameters: Target CPU (float, 3.90) · Target OTD % (95) · Target Damage % (0.45) · Target Days to Settle (40) · Amber band % (3) · Red band % (8) · Min lane units (5000) · Diesel change % (−30…40) · Volume growth % · Rate change % · Intermodal shift pts · Weight CPU/Spend/Units/OTD/Damage/DSO · Compare mode (WoW / YoY / vs Target).
Calcs: `CPU = SUM(spend_usd)/SUM(units)` · `CPU what-if = SUM(spend_usd*(1+fuel_share*[Diesel]/100)*(1+[Rate]/100))/SUM(units)` · `Var to target = ([CPU]-[Target CPU])/[Target CPU]` · `RAG = IF [Var]<=[Amber]/100 THEN 'On track' ELSEIF [Var]<=[Red]/100 THEN 'Watch' ELSE 'Off track' END` · `Attainment = MIN(1,[Target CPU]/[CPU])` (per KPI) · `Health = Σ weight×attainment / Σ weight` · `Prev period` via LOOKUP(…, −1) or −52 by compare mode.
Sheets: KPI tiles (6), Health gauge (two-bar donut), CPU trend vs target with what-if dot, Spend by mode bars, Lane watchlist table (filter units ≥ Min lane units). Dashboard 1360×1900 with parameter controls in a left panel.

## 2. Reader Growth Pulse — `subscriptions_weekly.csv` (+ `retention_curves.csv`)
Filters: plan, channel, market; parameter Window (4/13/26 weeks). Calcs: `Net adds = SUM(gross_adds)-SUM(churned)` · `Monthly churn = SUM(churned)/([Window]/4.33)/(SUM(active)+SUM(churned))` · `Retention health = 100-[Monthly churn]*800` · donut of gross_adds by channel · dual bars adds/churn with net-adds line (two sheets, no dual axis) · cohort heat map from `retention_curves` × plan mix · A/B card: two-proportion z-test as calcs on parameters p_control, p_variant, n.
Insight text: a worksheet with a single text mark whose calc concatenates strings by branch (e.g. `IF [Net adds chg]>=0.05 THEN 'Growth is accelerating…' …`).

## 3. Security Posture Scorecard — `security_team_domain.csv`
Parameters: six domain weights (0–50) · Job level (All/L4/L5/L6/L7+) · Quarter. Calcs: `Weighted score = SUM(score*weight×headcount)/SUM(weight×headcount)` · `Grade = IF s>=90 'A' ELSEIF >=80 'B' ELSEIF >=70 'C' ELSE 'D'` · Level adjustment `CASE [Job level] WHEN 'L4' THEN -3 … END`. Sheets: score dial, domain bars, radar (polygon via trig calcs: x = score×COS(angle), y = score×SIN(angle), path mark), bubble scatter (open_findings × median_days, size headcount, colour critical bucket), leaderboard table with level bars.

## 4. Hospital Margin & Quality — `hospital_quarterly.csv`
Filters: facility, service line, quarter. Calcs: `Op margin = (SUM(net)-SUM(opex))/SUM(net)` · waterfall via Gantt bar (running sum + negative size) · payer donut from share columns × net revenue (pivot the five share columns) · bullet charts vs benchmark parameters (readmission 15, HCAHPS 72, infection 1.0, mortality 2.8, LOS 4.9) · service-line bars with width = net revenue share (use a bar with size encoded or a Marimekko via table calcs).

Colour system: series blue #2a78d6, orange #eb6834, aqua #1baf7a, yellow #eda100; status green #008300 / amber #b97a00 / red #d0312f. Fonts: Tableau Book/Semibold.
