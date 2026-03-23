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
│   └── Sample - Superstore.csv         # Source dataset (9,994 rows)
│
├── output/
│   ├── SuperstoreClean.csv           # Cleaned & enriched transaction data
│   └── KPI.xlsx              # Aggregated KPI summary table
│
│
└── README.md
```
 
---
 
##  Workflow Architecture
<img width="1919" height="736" alt="image" src="https://github.com/user-attachments/assets/42f3a862-02da-4533-9e7d-7a628e07844f" />

 
The pipeline is structured in **phases**, each using Alteryx tools.
 
 
##  Tools Used

| Phase | Tool | Purpose |
|---|---|---|
| 1. Input | `Input Data` | Load raw CSV (9,994 rows, 21 columns) |
| 2. Parse Dates | `DateTime` ×2 | Parse `Order Date` and `Ship Date` from MM/dd/yyyy format |
| 3. Cleanse | `Data Cleansing` | Remove nulls and trim whitespace across all fields |
| 4. Transform | `DateTime` ×2 | Extract `Order_Year` and `Order_Month` from parsed date |
| 4. Transform | `Formula` | Calculate business KPI fields and `Quality_Flag` |
| 5. Filter | `Filter` | Split clean (T) vs flagged (F) records into two streams |
| 5. Filter | `Browse` | flagged review records for inspection |
| 6a. Path 1 | `Output Data` | Export full clean CSV
| 6b. Path 2 | `Summarize` | Group by Region, Category, Year - aggregate sales KPI |
| 6b. Path 2 | `Formula` | Round averaged KPI values |
| 6b. Path 2 | `Output Data` | Export KPI summary to Excel |
| 6b. Path 2 | `Sort` | Rank aggregated segments by Total Profit descending |
| 6b. Path 2 | `Sample` | Isolate Top 10 performing segments |
| 6b. Path 2 | `Browse` | Preview Top 10 ranked results on canvas |
 
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
