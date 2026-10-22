# Supplier Quality Power BI Analysis

## Project Overview

This project analyzes supplier quality performance using **Power BI, Python, and Excel** to identify the vendors, plants, materials, and defect types associated with production downtime and quality issues.

The analysis focuses on two years of supplier-quality records, from **2013 to 2014**, and investigates not only the overall change in downtime but also whether the increase is driven by vendor mix, recurring suppliers, specific defect categories, or extreme observations.

The project demonstrates an end-to-end analytics workflow including:

- Data quality validation
- Dimensional data modeling
- Python exploratory analysis
- Vendor performance analysis
- Defect and downtime analysis
- Sensitivity testing
- Power BI dashboard development
- Business insight generation

---

## Business Questions

The analysis was designed to answer the following questions:

- How did supplier-related production downtime change from 2013 to 2014?
- Is the increase caused by existing vendors or by changes in the vendor population?
- Which vendors contributed the most downtime in 2014?
- Which vendors experienced the largest deterioration in performance?
- Which plants experienced the highest downtime?
- Which material types were associated with the most downtime?
- Which defect types contributed most to the increase in downtime?
- Are defect quantity and downtime telling the same supplier-performance story?
- How concentrated is downtime among the highest-impact vendors?
- Are the vendor rankings sensitive to extreme downtime observations?

---

# Dataset

The project uses the Excel workbook:

```text
data/Supplier_Quality_PowerBI_Data.xlsx
```

The workbook contains a central metrics table and several supporting dimension tables.

## Dataset Summary

- **6,145 supplier-quality observations**
- **318 vendors observed in the fact data**
- **24 manufacturing plants**
- **22 material types**
- **6 product sub-categories**
- **3 defect types**
- Date range: **January 1, 2013 – December 31, 2014**
- Total recorded defect quantity: **56,010,955 units**
- Total recorded production downtime: **139,288 minutes**

---

# Data Structure

The source workbook contains the following sheets:

- `Metrics`
- `Category`
- `Date`
- `Defect`
- `Defect Type`
- `Material Type`
- `Plant`
- `Vendor`

The `Metrics` table functions as the central fact table, while the remaining tables provide descriptive attributes for analysis.

---

# Data Quality & Preparation

The accompanying Python notebook performs a structured validation process before analysis.

Key preparation steps include:

- Loading all fact and dimension tables from the Excel workbook
- Converting Excel serial dates into standard datetime values
- Checking missing values
- Auditing exact duplicate observations
- Validating dimension-table primary keys
- Checking for orphan foreign keys
- Validating date coverage
- Joining fact records to dimensions using many-to-one joins
- Confirming that joins preserve the original fact-table row count
- Creating year-level and vendor-level analytical summaries
- Testing sensitivity to extreme downtime observations

## Data Quality Findings

- Core analytical fields contain **no missing values**
- Dimension keys contain **no missing keys**
- No orphan Vendor, Plant, Defect, Defect Type, Material Type, Category, or Date keys were identified
- All analytical joins preserved the original **6,145 fact rows**
- **193 exact duplicate rows beyond the first occurrence** were identified

The duplicate observations were retained because the dataset does not contain an event-level unique identifier. Identical records therefore cannot be confirmed as erroneous duplicates.

---

# Data Model

The Power BI report uses a dimensional model centered on the supplier-quality metrics table.

The analytical structure can be represented as:

```text
                    Date
                     │
                     │
Vendor ─────────── Metrics ─────────── Plant
                     │
                     │
                  Defect
                     │
              Defect Type
                     │
              Material Type
                     │
                  Category
```

Supporting Power BI tables include dedicated tables for:

- Vendors
- Plants
- Dates
- Defects
- Defect Types
- Material Types
- Categories
- Measures

This structure enables consistent filtering and KPI calculation across the report.

---

# Dashboard Pages

The Power BI report contains three analytical pages.

---

## 1. Executive Overview

The Executive Overview provides a high-level summary of supplier quality performance.

Key metrics and analyses include:

- Total Downtime
- 2014 Downtime
- Downtime Change %
- Number of Vendors
- Top 5 Vendor Downtime Share
- Year-over-Year Downtime Comparison
- Vendor Concentration
- Material Type Analysis
- Defect Type Analysis
- Year Filters
- Vendor Filters

![Executive Overview](images/executive-overview.png)

---

## 2. Vendor Analysis

The Vendor Analysis page focuses on supplier-level performance and year-over-year change.

Key analyses include:

- Vendor Downtime in 2013
- Vendor Downtime in 2014
- Absolute Downtime Change
- Vendor Change %
- Common Vendors
- New Vendors in 2014
- Top Vendors by Downtime
- Largest Vendor Deterioration
- Vendor Performance Comparison
- Top 5 Vendor Concentration

![Vendor Analysis](images/vendor-analysis.png)

---

## 3. Defect Analysis

The Defect Analysis page focuses on operational drivers of downtime.

Key analyses include:

- Downtime by Defect Type
- Rejected Downtime
- Rejected Downtime Change
- Impact Downtime
- No Impact Downtime
- Defect Type Contribution
- Downtime by Plant
- Downtime by Material Type
- Year-over-Year Defect Performance
- Contribution to Net Downtime Increase

![Defect Analysis](images/defect-analysis.png)

---

# Python Analysis

The analytical notebook is available at:

```text
notebooks/supplier_quality_analysis.ipynb
```

The notebook performs:

- Data inspection
- Missing-value checks
- Duplicate audits
- Primary-key validation
- Foreign-key validation
- Date conversion
- Dimensional joins
- Annual downtime comparison
- Like-for-like vendor analysis
- Vendor ranking
- Vendor deterioration analysis
- Plant analysis
- Material analysis
- Defect-type analysis
- Defect quantity vs. downtime comparison
- Outlier detection
- Sensitivity analysis
- Chart generation
- Analytical output export

---

# Key Power BI Measures

The Power BI report includes measures such as:

```text
Total Downtime
Downtime 2014
Downtime Change %
Top 5 Downtime Share 2014
Vendor Count 2014
Vendor Change
Vendor Change %
Common Vendor Count
Common Vendor Flag
New Vendors 2014
No Impact Change %
```

The report also includes display measures designed specifically for KPI cards and dashboard storytelling.

---

# Key Insights

## 1. Overall downtime increased substantially

Recorded production downtime increased from:

- **62,681 minutes in 2013**
- to **76,607 minutes in 2014**

This represents an increase of:

**13,926 minutes (+22.22%)**

---

## 2. Vendor population growth explains part, but not all, of the increase

The number of observed vendors changed significantly:

- 2013: **162 vendors**
- 2014: **287 vendors**
- Vendors appearing in both years: **131**
- 2013-only vendors: **31**
- 2014-only vendors: **156**

To separate vendor-mix effects from supplier deterioration, a like-for-like analysis was performed on the 131 vendors present in both years.

For these common vendors:

- 2013 downtime: **61,446 minutes**
- 2014 downtime: **69,939 minutes**

Like-for-like downtime therefore increased by:

**8,493 minutes (+13.82%)**

This indicates that vendor population growth explains part of the overall 22.22% increase, but recurring vendors also deteriorated.

---

## 3. Downtime is concentrated among a small number of suppliers

The top five vendors accounted for approximately:

**41.02% of total 2014 downtime**

This indicates significant supplier concentration and suggests that improvement programs targeted at a small number of vendors could have a meaningful operational impact.

---

## 4. Reddoit remained the highest-downtime vendor

**Reddoit** recorded the highest supplier downtime in 2014:

**10,866 minutes**

However, Reddoit's downtime declined from **15,319 minutes in 2013**, representing an improvement of approximately **29.1%**.

This is an important distinction: the supplier with the highest current downtime is not necessarily the supplier deteriorating most rapidly.

---

## 5. J-lax recorded the largest deterioration among recurring vendors

Among vendors observed in both years, **J-lax** recorded the largest absolute increase:

- 2013: **300 minutes**
- 2014: **3,018 minutes**
- Increase: **2,718 minutes**

This represents an increase of approximately:

**906%**

**Planethouse** recorded the second-largest increase:

**+2,586 minutes**

These suppliers deserve attention even though their total downtime remains below Reddoit.

---

## 6. Rejected defects were the main deterioration driver

Downtime associated with `Rejected` defects increased from:

- **5,412 minutes in 2013**
- to **23,103 minutes in 2014**

This represents:

**+17,691 minutes (+326.88%)**

At the same time:

- `Impact` downtime decreased by approximately **3.0%**
- `No Impact` downtime decreased by approximately **58.9%**

Therefore, the rise in `Rejected` downtime more than explains the portfolio's net increase, while improvements in the other defect categories partially offset the deterioration.

---

## 7. Several plants represent operational hotspots

The plants with the highest recorded downtime in 2014 included:

1. **Skokie, IL — 8,442 minutes**
2. **Bangor, MI — 8,401 minutes**
3. **Cincinnati, OH — 8,338 minutes**

These locations represent potential priorities for plant-level root-cause analysis.

---

## 8. Corrugate was the highest-downtime material type

In 2014, `Corrugate` recorded approximately:

**26,308 minutes of downtime**

This was substantially higher than the next major material categories and suggests that supplier-quality interventions should consider material type in addition to supplier identity.

---

## 9. Defect quantity and downtime should be evaluated separately

The supplier rankings based on total defect quantity do not perfectly match the rankings based on production downtime.

This suggests that a supplier may generate many defects without causing the greatest operational disruption, while another supplier may generate fewer defects but create significantly more downtime.

For supplier management, both metrics should therefore be monitored rather than using defect quantity alone.

---

# Sensitivity Analysis

The notebook also tests whether supplier rankings are driven by extreme downtime observations.

The **99th percentile** of individual downtime records is:

**360 minutes**

There are **59 observations above this threshold**.

When observations above the 99th percentile are removed:

- Some vendor rankings change
- The concentration pattern becomes less extreme
- **Reddoit remains the highest-downtime vendor**

This provides additional evidence that the main supplier-risk conclusion is not driven entirely by a small number of extreme records.

---

# Business Recommendations

Based on the analysis, the following actions should be considered:

### 1. Prioritize high-downtime vendors

Supplier improvement programs should initially focus on vendors responsible for the largest share of total downtime.

Because the top five vendors account for approximately 41% of 2014 downtime, targeted intervention could produce substantial operational improvement.

### 2. Separate current severity from deterioration

Suppliers should be monitored using two different indicators:

- Current total downtime
- Year-over-year downtime change

For example, Reddoit remains the largest downtime contributor but is improving, while suppliers such as J-lax and Planethouse are deteriorating rapidly.

### 3. Investigate rejected defects

The sharp increase in downtime associated with rejected defects should be treated as the primary quality issue requiring root-cause investigation.

### 4. Focus on plant and material interactions

High-downtime plants and material types should be cross-analyzed with suppliers and defect types to identify whether specific supplier–plant–material combinations are creating recurring problems.

### 5. Monitor both defects and operational impact

Defect quantity alone should not determine supplier priority.

Supplier scorecards should combine:

- Defect quantity
- Production downtime
- Trend deterioration
- Defect severity
- Concentration of impact

---

# Tools Used

- **Power BI**
- **Power Query**
- **DAX**
- **Python**
- **Pandas**
- **NumPy**
- **Matplotlib**
- **Excel**
- **Jupyter Notebook / Google Colab**
- **Git**
- **GitHub**

---

# Skills Demonstrated

This project demonstrates practical skills in:

- Data quality assessment
- Data cleaning
- Data validation
- Dimensional modeling
- Star-schema design
- Fact and dimension tables
- Power Query
- DAX measure development
- Python data analysis
- Pandas
- Exploratory data analysis
- Year-over-year analysis
- Like-for-like comparison
- Vendor performance analysis
- Supplier quality analysis
- Defect analysis
- Plant analysis
- Material analysis
- Outlier detection
- Sensitivity analysis
- KPI design
- Data visualization
- Dashboard development
- Business insight generation
- Git version control
- GitHub documentation

---

# Project Workflow

```text
Supplier Quality Excel Dataset
            │
            ▼
       Data Validation
          Python
            │
            ▼
     Data Preparation
            │
            ▼
    Dimensional Modeling
            │
            ▼
   Exploratory Analysis
          Python
            │
            ▼
     Sensitivity Tests
            │
            ▼
     Power BI Measures
            │
            ▼
   Dashboard Development
            │
            ▼
     Business Insights
            │
            ▼
      GitHub Portfolio
```

---

# Project Structure

```text
supplier-quality-powerbi-analysis/
│
├── data/
│   └── Supplier_Quality_PowerBI_Data.xlsx
│
├── images/
│   ├── executive-overview.png
│   ├── vendor-analysis.png
│   └── defect-analysis.png
│
├── notebooks/
│   └── supplier_quality_analysis.ipynb
│
├── powerbi/
│   └── Supplier Quality Analysis.pbix
│
├── .gitignore
│
└── README.md
```

---

# How to View the Project

## Option 1: View Dashboard Screenshots

Dashboard screenshots are available in:

```text
images/
```

## Option 2: Open the Power BI Dashboard

Download:

```text
powerbi/Supplier Quality Analysis.pbix
```

and open the file using **Microsoft Power BI Desktop**.

## Option 3: Review the Python Analysis

Open:

```text
notebooks/supplier_quality_analysis.ipynb
```

to review the data validation, analysis, charts, and sensitivity testing.

## Option 4: Explore the Dataset

The source workbook is available at:

```text
data/Supplier_Quality_PowerBI_Data.xlsx
```

---

# Repository Purpose

This repository was created as a **Data Analytics portfolio project** to demonstrate practical skills in Power BI, Python, supplier performance analysis, data validation, dimensional modeling, and business intelligence.

The project emphasizes an important analytical distinction between **current supplier impact** and **supplier deterioration over time**, while also demonstrating how vendor mix, defect severity, plant location, material type, and extreme observations can affect operational conclusions.

---

# Author

**Binh Phan**

GitHub: [BinhPhan172](https://github.com/BinhPhan172)