# NorthBridge Health Investment Fund — Capstone B: The Investment Case

**Dataverse Africa Healthcare Data Analytics Cohort 4.0**
**Analyst:** Faith Chuwang-Kwa | **Track:** Healthcare Data Analytics | **Date:** May 2026

---

## Project Overview

This capstone project evaluates three hospital candidates on behalf of a fictional private equity firm — **NorthBridge Health Investment Fund** — to determine which hospital represents the strongest investment opportunity.

**Hospitals Evaluated:**
- Eastpoint Community Hospital (ECH) — Secondary facility, 190 beds
- Greenfield General Hospital (GGH) — Secondary facility, 280 beds
- Riverside Medical Centre (RMC) — Tertiary facility, 480 beds

**Data:** 10,900 patient encounters | January 2022 – December 2023 | 3 hospital datasets

**Recommendation:** **Greenfield General Hospital (GGH)** — Weighted Score: 7.23/10

---

## Repository Structure

```
northbridge-investment-case/
│
├── sql/
│   ├── ech_cleaning.sql              # ECH data cleaning script
│   ├── ggh_cleaning.sql              # GGH data cleaning script
│   ├── rmc_cleaning.sql              # RMC data cleaning script
│   ├── hospitals_combined.sql        # UNION ALL script to combine all three hospitals
│   └── metric_derivation.sql         # SQL queries for all 18 derived metrics
│
├── data/
│   └── hospitals_combined.csv        # Final clean dataset (10,900 rows, 39 columns)
│
├── excel/
│   └── NorthBridge_Investment_Scorecard.xlsx   # Weighted investment scorecard
│
├── powerbi/
│   └── NorthBridge_Investment_Dashboard.pbix   # Power BI dashboard (4 pages)
│
├── presentation/
│   └── NorthBridge_Investment_Presentation.pptx  # 11-slide PowerPoint presentation
│
├── recommendation/
│   └── NorthBridge_Investment_Recommendation.docx  # 1-2 page written recommendation
│
└── README.md
```

---

## Analytical Pipeline

### Stage 1 — Data Cleaning (SQL / PostgreSQL)
Raw data from three hospital sources was cleaned in PostgreSQL. Six categories of issues were identified and resolved per hospital:

- Inconsistent casing in `admission_type`, `claim_status`, `department`, `patient_gender`
- Non-standardised `patient_id` prefixes (added ECH-, GGH-, RMC- prefixes)
- Inconsistent `provider_name` values (standardised using `provider_id` as key)
- Derived `mortality_flag` from `outcome = 'Expired'` for GGH and RMC
- Standardised date formats across all three datasets
- Validated and combined all three datasets into `hospitals_combined` (10,900 rows)

### Stage 2 — Metric Derivation (SQL)
18 key performance indicators were derived from the cleaned dataset:

| Category | Metrics |
|---|---|
| Financial | Total Billed, Total Paid, Collection Rate, Denial Rate, Revenue per Encounter |
| Operational | Avg LOS, Avg ED Wait Time, Encounters per Bed |
| Clinical Quality | Mortality Rate, Readmission Rate, Complication Rate, HAI Rate |
| Patient Experience | Avg Satisfaction Score, Low Satisfaction Rate |
| Growth | Total Encounters, Quarterly Volume Trend, Encounter Distribution by Age Group |

### Stage 3 — Investment Scorecard (Excel)
A weighted scoring framework was applied across five investment dimensions using min-max scaling (1–10):

| Dimension | Weight | ECH | GGH | RMC |
|---|---|---|---|---|
| Financial Performance | 30% | 6.03 | 3.25 | 7.75 |
| Operational Efficiency | 25% | 7.66 | 8.42 | 1.00 |
| Clinical Quality | 25% | 6.50 | 10.00 | 1.00 |
| Patient Experience | 10% | 6.82 | 10.00 | 1.00 |
| Growth & Scalability | 10% | 6.00 | 6.50 | 7.50 |
| **Weighted Total** | | **6.63** | **7.23** | **3.67** |

### Stage 4 — Power BI Dashboard
A 4-page interactive dashboard was built in Power BI using a star schema data model:

**Data Model:**
- `Fact_Encounters` — core transactional table (10,900 rows)
- `Dim_Hospital` — hospital attributes
- `Dim_Patient` — patient demographics
- `Dim_Provider` — provider information
- `Dim_Diagnosis` — diagnosis and procedure codes
- `Dim_Date` — full date table with time intelligence support

**Dashboard Pages:**
1. **Executive Overview** — High-level investment snapshot with recommendation
2. **Financial Performance** — Revenue, collections, denial rate and claims analysis
3. **Clinical & Operational** — Quality metrics, severity distribution, discharge outcomes
4. **Experience & Growth** — Satisfaction scores, volume trends, demographic breakdown

### Stage 5 — Recommendation
A 1-2 page written investment recommendation was prepared for the NorthBridge Investment Committee summarising the analytical findings and investment thesis.

---

## Key Findings

### Why GGH?
GGH ranks **#1 in 8 of 12 performance metrics** — leading across all clinical quality and operational efficiency indicators:

- Lowest mortality rate: **2.64%** (vs ECH 4.37%, RMC 6.16%)
- Lowest readmission rate: **9.39%** (vs ECH 14.11%, RMC 19.66%)
- Fastest ED wait time: **33.5 minutes** (vs ECH 45.9 mins, RMC 61.1 mins)
- Shortest average LOS: **3.18 days** (vs ECH 3.79 days, RMC 5.03 days)
- Highest patient satisfaction: **7.68/10** (vs ECH 6.92, RMC 5.53)

GGH's sole weakness is a **50.58% claim denial rate** — a revenue cycle management problem, not a clinical one. Fixing this is estimated to unlock **$3.2M+ in additional annual revenue**.

### Why Not ECH?
Stable but no standout strength. 28.77% denial rate limits revenue. Lowest revenue per encounter ($1,163) with no clear growth catalyst.

### Why Not RMC?
Financially strongest but clinically and operationally weakest. Highest readmission rate (19.66%), slowest ED wait (61.1 mins), and 32.47% of patients rated satisfaction below 5/10.

---

## Technical Stack

| Tool | Purpose |
|---|---|
| PostgreSQL | Data cleaning and metric derivation |
| Microsoft Excel | Investment scorecard and weighted scoring |
| Microsoft Power BI | Interactive dashboard |
| Microsoft PowerPoint | Presentation |
| Microsoft Word | Written recommendation |

---

## Assumptions & Limitations

- **Readmission Rate:** Derived from `outcome = 'Readmitted'` as complete patient history for a standard 30-day readmission calculation was not available in the dataset.
- **Mortality Flag:** Derived from `outcome = 'Expired'` for GGH and RMC, validated against ECH where both the flag and outcome fields were confirmed identical.
- **Revenue Estimates:** The $3.2M revenue unlock estimate is based on applying RMC's denial rate (14.55%) to GGH's claim volume — this is an approximation and actual results would depend on payer mix, billing complexity, and implementation quality.
- **Data Period:** Analysis covers January 2022 to December 2023. All findings reflect this 24-month window only.

---

## About

This project was completed as part of the **Dataverse Africa Healthcare Data Analytics Internship — Cohort 4.0**.

**Analyst:** Faith Chuwang-Kwa
**Track:** Healthcare Data Analytics
**Cohort:** 4.0
**Date:** May 2026
