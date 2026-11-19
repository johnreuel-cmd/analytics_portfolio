# Northstar Digital Services — Workforce Investment & Economics Analytics

**Executive Decision Question:** *Where should Northstar allocate its next workforce investment: hiring, retention, or both?*
- **Pillar 1:** Where hiring investment is needed to support growth.
- **Pillar 2:** Where retention investment can reduce costly employee replacement.
- **Pillar 3:** Where targeted workforce action delivers the greatest business impact.

![Northstar Workforce Economics v2.1 Semantic Model ERD](docs/Northstar_Workforce_Gold_ERD_v2.svg)

---

## Executive Summary & Capital Allocation Recommendation (Rolling 12M: Oct 2025 – Sep 2026)

Across **1,178 active employees** and **₱1,247.8M** in annual loaded payroll, Northstar incurred **₱148.6M in Controllable Workforce Leakage (11.9% of payroll)**—comprising **₱80.7M** in turnover replacement costs (265 exits; 83 regrettable) and **₱67.9M** in active vacancy drag—alongside **₱29.6M** in separate Growth Hiring investment.

**Verdict: Targeted Both — Plug High-Cost Retention Leaks First, Then Fund Selective Growth Sourcing**

| Department | Active / Plan HC (Deficit) | Payroll (R12M) | Ret. Spend % | Ann. Total / Regrettable Attrition | Turnover / Vacancy Drag | Leakage % Payroll | Avg TTF (SLA) | Recommended Allocation Mandate |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| **Engineering** | 338 / 351 (3.7%) | ₱469.9M | 3.24% | 26.6% / **7.3%** (≤5.0%) | ₱39.2M / ₱33.0M | **15.4%** | 46.3 wd (40) | **Both (Retain First):** Halt 18–36m poaching cliff & close 6 critical backfills |
| **Sales & Client Success** | 253 / 270 (6.3%) | ₱254.6M | 3.05% | 20.8% / **7.3%** (≤6.0%) | ₱15.2M / ₱17.3M | **12.8%** | 36.6 wd (34) | **Split-Grade:** Fix 19.4% Job-Board SDR 90d churn; fast-track Ent AE growth |
| **Product & Design** | 192 / 214 (**10.3%**) | ₱239.1M | 2.69% | 12.4% / 2.2% (≤4.5%) | ₱8.2M / ₱12.6M | 8.7% | **55.4 wd** (42) | **Invest in Hiring:** Add niche PM/UX sourcing capacity; retention is healthy |
| **Operations & Delivery** | 271 / 277 (2.2%) | ₱168.8M | 1.51% | 34.5% / **13.1%** (≤5.5%) | ₱15.2M / ₱4.4M | 11.6% | 22.7 wd (26) | **Targeted Retention:** Coach 3 overloaded teams (`35+` span = 97.1% of reg. exits) |
| **People & Finance** | 124 / 124 (0.0%) | ₱115.3M | 2.49% | 9.8% / 1.6% (≤4.0%) | ₱2.9M / ₱0.6M | 3.0% | 23.2 wd (28) | **Maintain Baseline:** Healthy control group; redeploy excess budget |
| **Company Total** | **1,178 / 1,236 (4.7%)** | **₱1,247.8M** | **2.79%** | **22.8% / 7.1%** | **₱80.7M / ₱67.9M** | **11.9%** | **38.6 wd** | **₱148.6M Controllable Leakage** (+₱29.6M separate Growth Hiring) |

### Key Analytical Insights & Trade-Offs
1. **Separating Strategic Growth from Operational Loss:** Growth hiring (`₱29.6M`) is tracked independently from Controllable Leakage (`₱148.6M`) so leadership is never misled into "saving money" by freezing approved expansion.
2. **Engineering's 18–36 Month Poaching Cliff:** 90-day new-hire attrition in Engineering is just `1.1%` (proving strong recruiting quality), but Mid and Senior engineers exit at `1.5–3.0` years for external comp jumps—creating a backfill cascade behind `6` critical open seats (`>60` calendar days).
3. **Sales Split-Grade Trade-Off:** Aggregate Sales metrics mask two opposite problems—Junior SDRs hired via Job Boards suffer a **19.4% 90-day early exit rate** (onboarding/screening gap), while Senior Enterprise AEs retain well once ramped (`2.0%` regrettable) but sit unfilled behind a `6.3%` department deficit.
4. **Manager-Localized Risk in Operations:** Rather than funding an expensive department-wide wage hike across 271 Operations staff, manager-level slicing reveals that **97.1% of Operations regrettable exits** cluster in just **3 overloaded teams (`35+` span of control)** in Cebu and Cavite.
5. **Channel Economics:** Internal Mobility (`₱15.1K` cash recruiting fee, `24.9` working days to fill) and Employee Referrals (`₱35.5K`) dramatically outperform Contingency Agencies (`₱249.2K`, `57.2` working days).

---

## Architecture & Engineering Standards

### 1. CFO-Grade Accounting & Governance Rules
- **3-Way Financial Cost Nature Toggle (`_Param_Cost_Nature`):** Allows Finance and HR leaders to view every metric under **All Inclusive** (Cash + Ramp Productivity Deficit + Net Opportunity Risk), **Cash + Productivity Deficit**, or **Hard Cash Outlay Only** (strict P&L impact).
- **Working-Day & Active-Only Vacancy Burn:** Calculated strictly on Philippine working days (excluding weekends and 2022–2026 Philippine regular holidays) and strictly for active requisitions (`Open` / `Offer Extended`). All **922 `On Hold` weekly snapshots** carry **₱0.00** burn, and Support roles carry **₱0.00** opportunity risk.
- **Automatic Period Annualization & Uncensored Cohorts:** Period slicers (`Rolling 12M`, `YTD 2026`, `Q3 2026`) annualize attrition rates and simulator baselines automatically so partial quarters never trigger false-green alerts. Early attrition (`90d` / `12m`) divides strictly by uncensored eligible cohorts.
- **RA 10173 Privacy Compliance:** Zero PII columns; dates truncated to `YYYY-MM`; universal DAX suppression masks any breakdown where `Active Headcount < 5` or `Exits < 3`.

### 2. Multi-Grain Star/Snowflake Semantic Model (16 Tables • 31 `1:*` Joins)
Designed for zero Power Query transformation and zero dual-path ambiguity:

| Table | Type | Rows | Cols | Grain & Analytical Role |
| :--- | :--- | ---: | ---: | :--- |
| `dim_date` | Dimension | 1,826 | 17 | Daily calendar (2022–2026) with PH holiday & working-day flags |
| `dim_department` | Dimension | 5 | 7 | 5 departments with annual attrition, TTF SLA, and leakage targets |
| `dim_job_role` | Dimension | 32 | 15 | 5 depts × 4 grades with 2026 PH pay bands, ramp ranges & daily cover/margin |
| `dim_location` | Dimension | 4 | 6 | Metro Manila, Cebu, Cavite, Remote with regional pay differentials |
| `dim_employment_type` | Dimension | 3 | 6 | Regular, Probationary, Contractual with statutory load multipliers |
| `dim_manager_team` | Dimension | 38 | 6 | Anonymized team cohorts with verified span-of-control bands |
| `dim_performance_rating` | Dimension | 4 | 4 | 4 performance tiers (`Exceeds Expectations` = HiPo) |
| `dim_tenure_band` | Dimension | 4 | 5 | `<90 days`, `<1 yr`, `1-3 yrs`, `3+ yrs` |
| `dim_employee_profile` | Dimension | 2,473 | 7 | RA 10173 anonymized roster (`1,178` Active, `1,295` Separated, `200` internal hires) |
| `dim_requisition` | Dimension | 1,827 | 9 | Growth & Backfill requisition lifecycle (`1,769` Closed, `58` Open/Hold) |
| `dim_scenario_assumptions` | Parameter | 3 | 9 | Disconnected Baseline, Optimistic & Conservative saturation curve parameters |
| `fact_headcount_plan_monthly` | Snapshot | 7,680 | 9 | Month-End × Role × Site approved headcount plans & independent budgets |
| `fact_employee_monthly` | Snapshot | 58,301 | 15 | Month-End × Active Employee (2026 PH employer statutory burden & retention spend) |
| `fact_hiring` | Transaction | 1,769 | 21 | Completed hires with cash recruiting, onboarding, ramp deficit & cohort flags |
| `fact_vacancy_weekly` | Snapshot | 12,178 | 16 | Friday ATS operational snapshots tracking working-day active vacancy drag |
| `fact_separation` | Transaction | 1,295 | 19 | Exit events with backfill replacement costs reconciled within `±8.97%` of hiring |

---

## Repository Structure

```text
northstar-workforce-economics/
├── README.md                                              # Executive summary, architecture & setup guide
├── docs/
│   ├── Northstar_Workforce_Dashboard_Blueprint_v2.pdf     # Executive requirements, 6-page layout & decision logic
│   ├── Northstar_Field_Mapping_Data_Dictionary_v2.pdf     # Complete 171-col dictionary, visual bindings & 72 DAX measures
│   ├── Northstar_Workforce_Gold_ERD_v2.svg                # Lossless vector Power BI Semantic Model ERD
│   └── Northstar_Workforce_Gold_ERD_v2.png                # High-resolution (4972×4052) ERD preview
├── etl/
│   ├── generate_northstar_gold_v2.py                      # Reproducible Python Gold-layer generator (seed=20260930)
│   └── test_northstar_reconciliation.py                   # 16-gate automated QA, stock-flow & realism test suite
├── data/
│   └── gold/                                              # 16 Power BI-ready Gold CSV tables
└── dashboard/
    └── northstar_workforce_v2_dashboard.html              # Interactive 6-page browser prototype & simulator
