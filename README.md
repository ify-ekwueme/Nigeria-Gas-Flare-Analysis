# Nigeria Gas-Flare Analysis (2012-2024)
Analyzed 13 years (2012-2024) of Nigeria's Gas Flaring data from the World bank GGFR to uncover emisssion trends, identify the highest contributing operators and fields, and estimate the economic and environmental cost of flared gas across fields and operators.


## ⚙️ Project Type 
- [x] Exploratory Data Analysis (EDA)
- [x] SQL Analysis / Querying
- [x] Dashboard / Data Visualization
- [x] Data Cleaning / Wrangling
- [x] End-to-End (multiple of the above)

---

## Table of Contents
1. [Project Overview](#1-project-overview)
2. [Objectives](#2-objectives)
3. [Project Scope & Tools](#3-project-scope--tools)
4. [Repository Structure](#4-repository-structure)
5. [Data Workflow](#5-data-workflow)
6. [Data Model & Schema](#6-data-model--schema)
7. [Analysis & Metrics](#7-analysis--metrics)
8. [Key Insights](#8-key-insights)
9. [Recommendations](#9-recommendations)
10. [Assumptions & Limitations](#10-assumptions--limitations)
11. [Future Enhancements](#11-future-enhancements)
12. [Dashboard](#12-dashboard)
13. [Author](#13-author)

---

## 1. Project Overview

  Nigeria has been one of the world's largest gas flaring nation for decades. However, the operators and fields driving the majority of that flaring remained poorly invisible in national-level reporting. 

### Problem Statement 
  This project explored 13 years of satellite verified flaring data from the World Bank GGFR across 210 fields and 55 operators to determine whether;
- Nigeria's flaring problem was improving overtime.
- Which operators and field types ere the most responsible.
- What the true economic cost flared gas has been betwwen 2012-2024.

### Outcome
  The analysis revealed that despite 45% decline in flaring volume between 2012 and 2022, flaring reversed course in 2023 and continued rising until 2024, and that just 3 operators accounted for 41% of all gas across the entire period, a concentration that is invisible in the country's top-line reporting.

---

## 2. Objectives
---
The core objective was to design a Power BI dashboard that transforms 13 years of World Bank GGFR satellite-verified gas flaring data into actionable insights in order to;

- **Primary Objective:** Enabling regulators, stakeholders, and energy analysts to track Nigeria's flaring trends, identify the highest-contributing operators and fields.
  
- **Secondary Objective 1:** Estimate the potential revenue lost due to gas flaring using Henry Hub natural gas prices, rather than relying on a single flat price assumption.
 
- **Secondary Objective 2:** Measure environmental impact by calculating emissions using the industry-standard conversion factor, and map field-level emission hotspots geographically using latitude and longitude coordinates. 

---

## 3. Project Scope & Tools

### Scope
  
| Dimension | Details |
|-----------|---------|
| **In Scope** | The gas flaring data extracted from the World Bank GGFR global dataset covering 2012–2024. Analysis covers total flaring volume in BCM, revenue loss, co2 emission, etc.|
| **Out of Scope** |Analysis of gas flaring data outside Nigeria; The global GGFR dataset was intentionally filtered to Nigeria only for this project.|
| **Out of Scope** | Nigeria-specific domestic gas prices were not available in the dataset; Henry Hub was used as a disclosed benchmark proxy, not as a precise representation of what operators received.|
| **Out of Scope** | Total gas produced per field was not available in the GGFR dataset, making it impossible to calculate what percentage of produced gas was flared at the field level. |
| **Time Period** | 2012-2024 |

### Tools & Technologies

| Purpose | Tool(s) Used |
|----------|-------------|
| Data Storage | CSV files |
| Data Processing |  SQL, Excel |
| Analysis | SQL queries, Powerbi (Dax) |
| Visualization | Power BI |
| Version Control | GitHub |

---

## 4. Repository Structure

```
Nigeria Gas Flare Analysis 2012-2024/
│
├── data/
│   ├── raw/
│        └── 2012-2024-Flare-Volume-Estiamte-by-individual-Flare-Location.xlsx
│   ├── processed/
│        └── flaring_cleaned.csv
│   └── external/
│       └── Henry_Hub_Prices.csv
│
│── docs/
│        └── flaring_filtered_documentation.xlsx
│
├── queries/               
│   ├── exploratory/
│        └── 03_eda_v1.sql
│   ├── transformations/
│       ├── 01_SQL_Overview_v1.sql
│       └──  02_profiling&cleaning_v1.sql
│   └── final/
│        └── flaring_filtered_sql.csv     
│
├── reports/              
│   ├── Gas flare Analysis 2012-2024.pdf
│
├── visuals/        
│   ├── Executive_Overview.jpeg
│    └── Geographical_Overview.jpeg
│
└── README.md                
```

---

## 5. Data Workflow

1. **Source:**
- Yearly CSV exports pulled from a single file containing satellite-verified gas flaring ,measurements from all countries, covering 2012-2024. It was accessed through the World bank open-data portal.

2. **Ingestion:**
- The global GGFR Excel file was downloaded and filtered to Nigeria only using Power query, extracting 2,264 rows from a 156,329-row global dataset .
- A supplementary Henry Hub annual gas price lookup table (2012–2024) was sourced separately from the **U.S. Energy Information Administration (EIA) through FRED** and loaded as a  CSV, then joined on the Year column in Powerbi.
 
3. **Cleaning:**
- Data was cleaned in SQL.
- Category columns  were standardised to resolve inconsistent labelling and casing.
- Resolved two different Operator naming inconsistencies (2 variants → 1 )
- Fixed unnecessary capitalizations in the dataset.
- Documented data quality issue.
   
4. **Transformation:**
- Defined calculated measures (Total flared vol, CO2 emission, AVG mmscfd, etc.).
- Compared the current year's flaring volume with the Previous Year to measure annual changes.
- Revenue loss (calculated row-by-row via SUMX using year-specific Henry Hub prices joined through a relationship on the Year column) on powerbi.

5. **Analysis:** 
- Performed exploratory SQL analysis and built Power BI dashboards to identify gas flare trends, fields with high flaring volumes emissions, and revenue loss.

6. **Output:**
A two-page Power BI dashboard

- An Executive Summary page featuring KPI cards (total BCM, revenue loss, CO₂ emissions, YoY change) alongside trend lines, operator ranking etc
- A Geographic Overview page featuring a bubble map of Nigeria's flaring hotspots sized by BCM and coloured by flare severity, top 10 fields bar chart and flare level distribution visual, all connected by synced slicers for Year, Operator, and Location filtering across both pages.
  
---

## 6. Data Model & Schema

### Dataset 
Table 1: `flaring_filtered`

| Field Name | Data Type | Description | Example Value |
|------------|-----------|-------------|---------------|
| country | text | country where flaring occurred | Nigeria |
| latitude | decimal | geograhical latitude of flare site | 4.67864 |
| longitude | decimal | [geographical longitude of flare site | 8.54667 |
| bcm | decimal | flaring volume in billion cubic metre | 131.06432 |
| mmscfd | decimal | million tandard cubic feet per day (industry flow rate) | 65.789 |
| year | int | year of flaring event | 2012 |
| field_type | text | oil/gas/lng field | Oil |
| field_name | text | name of oil/gas field or production  block | Usan |
| field_operator | text | name of petroleum operator responsible for the facility | TotalEnergies |
| location | text | onshore or offshore | Offshore |
| flare_level | text | qualitative intensity category of flaring | Medium |
| flaring_vol_million_m3 | decimal | volume of gas flared in million cubic metre (mm³)| 0.13106432 |

Table 2: `Henry_Hub_Pices.csv`
| Field Name | Data Type | Description | Example Value |
|------------|-----------|-------------|---------------|
| year | int | year of flaring event | 2012 |
| price | decimal | gas price usd per mmbtu | 2.21 |

---

**Table Relationships Summary:**

| Relationship  | Type |
|-------------|------|
| Flaring_filtered [`year`] →  Henry_hub [`year`]| one-to-many |

---

## 7. Analysis & Metrics

### Analytical Approach
This project followed an exploratory data analysis (EDA) approach to understand gas flaring trends in Nigeria.

### Key Metrics

| Metric | Description | Business value |
|--------|--------------------------|----------------|
| `Total gas flared` | Total volume of gas flared across Nigeria during the analysis period | Measures the scale of gas flaring |
| `Estimated Revenue Loss` | Estimated economic value of gas flared using hanry hub prices | Quantifies the financial impact of gas flaring |
| `CO₂ Emission` | Carbon dioxide emission gotten from flaring | Evaluates environmental impact|
| `YOY` | percentage change in gas flaring volume compared to previous year | Identifies increasing or decreasing flaring trends. |


### Methods Used

-  Trend analysis across 2012-2024
-  prepared the dataset in Excel by filtering the global dataset to Nigeria and documenting data quality issues.
-  SQL window functions and aggregations to answer key analytical questions
-  Developed DAX measures for business metrics, including Year-over-Year Growth, Total CO₂ Emissions, and Estimated Revenue Lost.
-  Built a Power BI dashboard to present key environmental and economic insights for stakeholders.

---

## 8. Key Insights

**Insight 1: Nigeria Gas Flaring Decline is Frugal Not Structural**

Nigeria reduced annual gas flaring from 9.62 BCM in 2012 to 5.32 BCM in 2022 (a 45% decline). However, flaring rebounded in 2023 (+8.8%) and 2024 (+12%), suggesting the earlier decline was likely driven by production disruption rather than the result of sustained infrastructure improvements or regulatory enforcement.

**Insight 2: Three Operators Hold the Key to Nigeria's Flaring Problem**

ExxonMobil, Eni, and Chevron contributed approximately 41% of Nigeria's total gas flaring between 2012 and 2024. This concentration suggests that targeted regulation and gas capture investments focused on these operators could deliver a greater national impact than broad industry-wide policies.

**Insight 3: Gas Price Volatility Makes Flat Revenue Loss Estimates Dangerously Misleading**

Integrating year-specific Henry Hub gas prices showed that the estimated revenue lost from gas flaring varied significantly over time. Gas prices ranged from $2.03/MMBtu (2020) to $6.45/MMBtu (2022), demonstrating that using a single gas price would misrepresent the true economic cost of flaring.

**Insight 4 : Offshore Flaring is an Equally Significant but Systematically Overlooked Problem** 

Gas flaring was almost evenly split between offshore (47.4%) and onshore (52.6%) operations. Despite this, policy and public attention have largely focused on onshore flaring, indicating that offshore emissions may be under-prioritized.

**Insight 5 : Flare Severity Classification Understates Where the Real Damage Is**

Medium-classified fields generated approximately 71% of total gas flaring despite representing only 28% of records, while small fields made up 72% of records but contributed just 28% of flaring. This suggests that prioritizing enforcement by the number of flagged fields rather than flaring volume may overlook the highest-impact emission sources.


---

## 9. Recommendations

| Priority | Recommendation | Based On |
|----------|---------------|----------|
| High | Prioritize gas capture investments for ExxonMobil, Eni, and Chevron, as these operators account for approximately 41% of Nigeria's total gas flaring. | insight 2 | 
| High | Adopt a volume-based flare severity framework so regulatory enforcement targets the highest-emitting fields rather than the most frequently flagged sites. | insight 5 | 
| High |  Investigate the 2023–2024 increase in gas flaring to identify whether the rebound was driven by production, infrastructure, or regulatory challenges before introducing new reduction targets | insight 1 | 
| Meduim | Standardize annual revenue loss reporting using year-specific gas prices to provide a more accurate estimate of the economic cost of gas flaring. | insight 3 | 


---

## 10. Assumptions & Limitations

### Assumptions
- Henry Hub annual average gas prices were used as a proxy to estimate the economic value of Nigeria's flared gas due to the absence of Nigeria-specific gas price data.
- The GGFR CO₂ emission factor (2.75 kg CO₂/m³) was applied consistently across all records.
- GGFR field locations, operator names, and classifications were assumed to be accurate.
- Annual average Henry Hub prices were applied uniformly to each year's flaring volume, without accounting for intra-year price fluctuations.
- Fields classified as "NA" were retained as a separate category without reclassification.

### Limitations
- The GGFR dataset does not include total gas production volumes, preventing calculation of flare ratios by field or operator.
- Revenue loss estimates are based on Henry Hub prices and may not reflect actual Nigerian domestic gas prices, representing an approximate estimate rather than realized values.
- Satellite-derived GGFR data may not capture very small or intermittent flaring events, potentially understating total flaring volumes.
- Environmental analysis focuses on CO₂ emissions and does not account for methane slip or other pollutants.

---

## 11. Future Enhancements
- Additional datasets (e.g., Nigerian gas prices, production volumes, and regulatory records) would improve the accuracy and depth of future analyses.
- Expand the Environmental Impact Model to Include Methane and Non-CO₂ Emissions


---

## 12. Dashboard

## Executive Overview

![Executive Overview](visuals/Executive_Overview.jpeg)

---

## Geographical Analysis

![Geographical Analysis](visuals/Geographical_Overview.jpeg)

---

## 13. Author

**Ekwueme Ifeoma**

Data Analyst - Aspiring Business intelligence Analyst]

- 🔗 [ www.linkedin.com/in/ifeoma-ekwueme]
- 📧 [amandaekwueme540@gmail.com]

---


