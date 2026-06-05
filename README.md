# NorthBridge Health Investment Fund — The Investment Case

A healthcare investment analysis evaluating three hospital candidates on behalf of a fictional private equity firm — NorthBridge Health Investment Fund. Built as part of the Dataverse Africa Healthcare Data Analytics Internship — Cohort 4.0.

---

## Table of Contents
- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning](#data-cleaning)
- [Metric Derivation](#metric-derivation)
- [Data Model](#data-model)
- [Investment Scorecard](#investment-scorecard)
- [Dashboard Preview](#dashboard-preview)
- [Key Findings](#key-findings)
- [Investment Recommendation](#investment-recommendation)
- [Limitations](#limitations)

---

## Project Overview

This project simulates a real-world private equity investment analysis. Acting as a Healthcare Data Analyst for NorthBridge Health Investment Fund, the objective was to:

- Clean and validate raw hospital encounter data from three separate sources
- Derive 18 key performance indicators across financial, clinical, operational, and experiential dimensions
- Build a star schema data model in Power BI
- Score each hospital across five investment dimensions using a weighted framework
- Build a four-page interactive Power BI dashboard that tells the investment story
- Deliver a written investment recommendation to the fictional investment committee

The analysis answers one core question:

> **Which of the three hospital candidates represents the strongest investment opportunity for NorthBridge Health Investment Fund?**

---

## Data Sources

- **Dataset:** Three separate hospital encounter datasets (one per hospital)
- **Combined Records:** 10,900 patient encounters
- **Period:** January 2022 — December 2023
- **Columns:** 39 fields per record covering patient demographics, clinical outcomes, financial claims, operational timestamps, and provider information

| Hospital | Code | Type | Beds | Encounters |
|---|---|---|---|---|
| Eastpoint Community Hospital | ECH | Secondary | 190 | 3,500 |
| Greenfield General Hospital | GGH | Secondary | 280 | 3,600 |
| Riverside Medical Centre | RMC | Tertiary | 480 | 3,800 |

---

## Tools

- **PostgreSQL / pgAdmin 4** — Data cleaning, metric derivation, and UNION ALL merge
- **Microsoft Excel** — Investment scorecard and weighted scoring framework
- **Microsoft Power BI** — Star schema data model, DAX measures, and 4-page dashboard
- **Power Query (M)** — Dimension table creation and calculated columns
- **Microsoft PowerPoint** — Investment presentation
- **Microsoft Word** — Written investment recommendation and project documentation

---

## Data Cleaning

Data cleaning was performed in PostgreSQL. Each hospital dataset was cleaned independently before being combined. Six categories of data quality issues were identified and resolved across all three datasets:

| Issue | Action Taken |
|---|---|
| Inconsistent casing in `admission_type`, `claim_status`, `department`, `patient_gender` | Standardised to title case using `INITCAP(LOWER(TRIM()))` |
| Missing hospital prefix on `patient_id` | Added ECH-, GGH-, RMC- prefixes to ensure uniqueness across combined dataset |
| Inconsistent `provider_name` entries — split names, truncated values, typos | Standardised using `provider_id` as the key via CASE statement |
| Missing or inconsistent `mortality_flag` for GGH and RMC | Derived from `outcome = 'Expired'`, validated against ECH where both fields matched |
| Mixed date formats across datasets | Cast `admission_date` and `discharge_date` to consistent DATE type |
| Leading and trailing whitespace in text columns | Applied `TRIM()` across all relevant text fields |

After cleaning, the three datasets were combined using `UNION ALL` into a single `hospitals_combined` table:

| Hospital | Row Count |
|---|---|
| ECH | 3,500 |
| GGH | 3,600 |
| RMC | 3,800 |
| **Total** | **10,900** |

---

## Metric Derivation

18 key performance indicators were derived from `hospitals_combined` using SQL:

| # | Metric | ECH | GGH | RMC |
|---|---|---|---|---|
| 1 | Total Billed | $9.0M | $8.0M | $11.6M |
| 2 | Total Paid | $4.1M | $2.3M | $6.6M |
| 3 | Collection Rate | 45.27% | 29.10% | 56.93% |
| 4 | Denial Rate | 28.77% | 50.58% | 14.55% |
| 5 | Revenue per Encounter | $1,163 | $644 | $1,744 |
| 6 | Mortality Rate | 4.37% | 2.64% | 6.16% |
| 7 | Readmission Rate | 14.11% | 9.39% | 19.66% |
| 8 | Complication Rate | 12.60% | 10.89% | 17.82% |
| 9 | HAI Rate | 4.29% | 3.58% | 5.58% |
| 10 | Average Length of Stay | 3.79 days | 3.18 days | 5.03 days |
| 11 | Average ED Wait Time | 45.95 mins | 33.50 mins | 61.11 mins |
| 12 | Encounters per Bed | 18.4 | 12.9 | 7.9 |
| 13 | Average Satisfaction Score | 6.92/10 | 7.68/10 | 5.53/10 |
| 14 | Low Satisfaction Rate | 7.17% | 2.31% | 32.47% |

---

## Data Model

A star schema was built in Power BI using Power Query:

**Fact Table:**
- `Fact_Encounters` — core transactional table (10,900 rows)

**Dimension Tables:**
- `Dim_Hospital` — hospital attributes
- `Dim_Patient` — patient demographics
- `Dim_Provider` — provider information
- `Dim_Diagnosis` — diagnosis and procedure codes
- `Dim_Date` — full date table built using DAX `CALENDAR()` function

All relationships are Many-to-One, single direction, from `Fact_Encounters` to each dimension table.

**Calculated Columns (Power Query):**
- `Age Group` — Paediatric / Young Adult / Middle-Aged Adult / Older Adult / Elderly
- `Severity Group` — Low / Medium / High / Critical

**DAX Measures:** 14 measures including Collection Rate, Denial Rate, Mortality Rate, Readmission Rate, Avg LOS, Avg ED Wait Time, Avg Satisfaction, Revenue per Encounter, Encounters per Bed, Low Satisfaction Rate, and more.

---

## Investment Scorecard

Each hospital was scored 1–10 per dimension using min-max scaling, then weighted by investment priority:

| Dimension | Weight | ECH | GGH | RMC |
|---|---|---|---|---|
| Financial Performance | 30% | 6.03 | 3.25 | 7.75 |
| Operational Efficiency | 25% | 7.66 | 8.42 | 1.00 |
| Clinical Quality | 25% | 6.50 | 10.00 | 1.00 |
| Patient Experience | 10% | 6.82 | 10.00 | 1.00 |
| Growth & Scalability | 10% | 6.00 | 6.50 | 7.50 |
| **Weighted Total** | | **6.63** | **7.23** | **3.67** |

---

## Dashboard Preview

### Page 1 — Executive Overview
<img width="1235" height="683" alt="Page 1 - EXECUTIVE OVERVIEW" src="https://github.com/user-attachments/assets/06ba7cb0-a82e-4f73-abb7-a2cad76e7713" />


### Page 2 — Financial Performance
<img width="1216" height="675" alt="Page 2 - FINANCIAL PERFORMANCE" src="https://github.com/user-attachments/assets/91e2b4ca-ec21-46cf-accf-4e47d7cb0567" />


### Page 3 — Clinical & Operational Performance
<img width="1225" height="680" alt="Page 3 - CLINICAL   OPERATIONAL PERFORMANCE" src="https://github.com/user-attachments/assets/6e8a7727-80bc-410a-a324-61ca303c6748" />


### Page 4 — Experience & Growth
<img width="1214" height="673" alt="Page 4 - EXPERIENCE   GROWTH" src="https://github.com/user-attachments/assets/18dd872b-8332-426a-8839-d95ef12e22ab" />


---

## Key Findings

1. **GGH ranks #1 in 8 of 12 performance metrics** — leading across all clinical quality and operational efficiency measures
2. **GGH's 50.58% denial rate** is the highest of the three hospitals, creating a $5.65M annual revenue gap — this is a billing problem, not a clinical one
3. **RMC is financially strongest today** (56.93% collection rate, $6.6M collected) but carries the highest readmission rate (19.66%), slowest ED response (61.1 mins), and the poorest patient satisfaction (5.53/10)
4. **1 in 3 RMC patients** rated their experience below 5 out of 10 — a serious reputational and patient retention risk
5. **ECH is stable but offers limited upside** — no standout strength in any dimension, no clear investment catalyst
6. **GGH's clinical foundation is exceptional** — lowest mortality (2.64%), lowest readmissions (9.39%), fastest ED response (33.5 mins), shortest LOS (3.18 days), highest satisfaction (7.68/10)
7. If GGH's denial rate were reduced to match RMC's (14.55%), the estimated additional annual revenue is **$3.2M+**

---

## Investment Recommendation

**Recommended Investment: Greenfield General Hospital (GGH) — Weighted Score: 7.23/10**

GGH already operates at the highest level where it matters most — patient safety, clinical quality, and operational efficiency. Its financial underperformance is caused by a single, addressable problem: a 50.58% claim denial rate. Targeted investment in revenue cycle management — billing processes, claims documentation, staff training — unlocks the full financial value of an already exceptional hospital.

**Why not ECH?** Stable with no critical weaknesses, but no standout strength and no clear upside. Low risk, low reward.

**Why not RMC?** Financially strongest today, but deep clinical and operational problems — highest readmissions, slowest ED, poorest satisfaction — would require expensive, uncertain, and long-term intervention to resolve.

---

## Limitations

- **Readmission Rate** was derived from `outcome = 'Readmitted'` rather than a clinical 30-day calculation, as the dataset does not contain complete patient history across visits. This approach was documented as an assumption.
- **Mortality Flag** for GGH and RMC was derived from `outcome = 'Expired'`. Validated against ECH where both the flag column and the outcome column produced identical results.
- **Revenue Unlock Estimate** of $3.2M is an approximation based on applying RMC's denial rate to GGH's claim volume. Actual results depend on payer mix, denial root causes, and implementation quality.
- **Data Period** covers January 2022 to December 2023 only — findings reflect this 24-month window and may not represent longer-term trends.
- **Dataset is simulated** for training purposes — real hospital data would contain additional complexity including ICD coding accuracy, payer contract variations, and staff-level performance data.

---

*Prepared by Faith Chuwang-Kwa | Healthcare Data Analytics — Cohort 4.0 | Dataverse Africa | 2026*
