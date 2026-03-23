# Superstore Sales ETL Pipeline
### Built with Alteryx Designer | Sales & Revenue Analytics

---
 
## Project Overview
 
This project demonstrates a **production-style ETL (Extract, Transform, Load) pipeline** built in Alteryx Designer using the Sample Superstore Sales dataset. The workflow ingests raw transactional sales data, cleanses and enriches it with business metrics, aggregates it into a KPI summary, and exports two reporting-ready outputs.
 
 
---
 
## Business Problem
 
A retail company collects transactional sales data across multiple regions, product categories, and customer segments. The raw data contains inconsistencies, missing values, and no derived KPIs. Business stakeholders need:
 
- A **clean, enriched dataset** ready for BI tools like Tableau or Power BI
- A **KPI summary** aggregated by Region, Category, and Year for executive reporting
- A **data quality log** of flagged records requiring review
 
---
 
## Repository Structure
 
```
alteryx-sales-etl/
│
├── workflow/
│   └── Sales_ETL_Pipeline.yxmd       # Alteryx workflow file
│
├── data/
│   └── Sample_Superstore.csv         # Source dataset (9,994 rows)
│
├── output/
│   ├── SuperstoreClean.csv           # Cleaned & enriched transaction data
│   └── KPI_Summary.xlsx              # Aggregated KPI summary table
│
├── screenshots/
│   └── workflow_overview.png         # Full canvas screenshot
│
└── README.md
```
 
---
 
## 🔧 Workflow Architecture
 
The pipeline is structured in **7 phases**, each using purpose-specific Alteryx tools:
 
```
[INPUT - Superstore]
    → [SELECT - Fix Types]
    → [CLEANSE - Nulls & Whitespace]
    → [FORMULA - Quality Flag]
    → [DATETIME - Extract Year & Month]
    → [FORMULA - Business Metrics]
    → [FILTER - Clean Records]
         ├── F → [BROWSE - Review Records]          Flagged data stream
         └── T → [OUTPUT - Clean Dataset]           Clean data stream
                  → [SUMMARIZE - KPIs by Region & Category]
                  → [FORMULA - Round Averages]
                  → [SORT - By Profit Desc]
                  → [SAMPLE - Top 10 Segments]
                  → [BROWSE - Top 10 Preview]
                  → [OUTPUT - KPI Summary]
```
 
---
 
## 🛠️ Tools Used
 
| Phase | Tool | Purpose |
|---|---|---|
| 1. Input | `Input Data` | Load raw CSV (9,994 rows, 21 columns) |
| 2. Cleanse | `Select` | Fix data types — dates, Postal Code as string |
| 2. Cleanse | `Data Cleansing` | Remove nulls, trim whitespace |
| 2. Cleanse | `Formula` | Add `Quality_Flag` column (OK / Review) |
| 3. Transform | `DateTime` | Extract `Order_Year` and `Order_Month` |
| 3. Transform | `Formula` | Calculate business metrics (see below) |
| 4. Filter | `Filter` | Split clean vs. flagged records into two streams |
| 5. Aggregate | `Summarize` | Group KPIs by Region + Category + Year |
| 5. Aggregate | `Formula` | Round average values for readability |
| 6. Rank | `Sort` | Order by Total Profit descending |
| 6. Rank | `Sample` | Isolate Top 10 performing segments |
| 7. Output | `Output Data` ×2 | Export clean CSV + KPI Excel file |
 
---
 
## Calculated Fields (Business Metrics)
 
| Field | Formula | Business Purpose |
|---|---|---|
| `Quality_Flag` | `IF Sales <= 0 OR Discount >= 1 THEN "Review" ELSE "OK"` | Data quality gate |
| `Order_Year` | `DateTimeYear([Order Date])` | Enables year-over-year analysis |
| `Order_Month` | `DateTimeMonth([Order Date])` | Enables seasonality analysis |
| `Days_to_Ship` | `DateTimeDiff([Ship Date], [Order Date], "days")` | Operational efficiency KPI |
| `Profit_Margin_Pct` | `ROUND(([Profit] / [Sales]) * 100, 2)` | Core financial performance KPI |
 
---
 
## 📊 Output Files
 
### 1. `SuperstoreClean.csv`
- **Rows:** 9,994
- **Columns:** 27 (21 original + 6 derived)
- **Use case:** Feed directly into Tableau, Power BI, or further SQL analysis
 
### 2. `KPI_Summary.xlsx`
- **Rows:** 48 (4 Regions × 3 Categories × 4 Years)
- **Columns:** 9 KPI metrics
- **Use case:** Executive reporting, dashboard data source
 
| Column | Description |
|---|---|
| `Region` | Sales region (West, East, Central, South) |
| `Category` | Product category (Furniture, Technology, Office Supplies) |
| `Order_Year` | Year of order (2014–2017) |
| `Sum_Sales` | Total revenue |
| `Sum_Profit` | Total profit |
| `Sum_Quantity` | Total units sold |
| `Avg_Profit_Margin_Pct` | Average profit margin % |
| `Avg_Days_to_Ship` | Average fulfillment time in days |
| `Total_Orders` | Count of distinct orders |
 
---
 
## Key Business Insights
 
- **West + Office Supplies (2017)** is the top profit segment at **$22,121**
- **Technology** consistently ranks as the highest-margin category in East and West
- **Furniture** shows negative or near-zero profit in several regions — a pricing or discount issue worth investigating
- Average shipping time is a consistent **4 days** across all regions and categories, indicating stable logistics operations
- Overall profit trend is upward from 2014 → 2017 across most segments
 
---
 
## Dataset
 
**Source:** [Sample Superstore Dataset — Kaggle (vivek468)](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final)
 

---
 
## Author
 
**Diego Martinez Zubillaga**
BS in Computer Science & Business Analytics
Alteryx Designer Certified

www.linkedin.com/in/diego-martinez-zubillaga

 
---
 
*This project is part of a growing collection of Alteryx and BI workflows. More coming soon.*
