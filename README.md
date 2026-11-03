# Blinkit Sales Dashboard (Power BI)

An interactive Power BI dashboard analysing sales performance, customer ratings, and outlet distribution for Blinkit grocery data. It covers 8,523 item records across 10 outlets and answers questions such as which item types sell most, how fat content splits sales, and how outlet size, location, and type affect revenue.

## Dashboard Preview

![Blinkit Sales Dashboard](images/dashboard.png)

## Business Objective

Analyse Blinkit's sales performance, customer satisfaction, and inventory distribution to find insights and optimisation opportunities using KPIs and visualisations in Power BI.

## KPIs

| KPI | Definition | Value |
|---|---|---|
| Total Sales | Sum of `Sales` across all records | ~$1.2M |
| Avg Sales | Average `Sales` per record | ~$141 |
| No of Items | Count of item records (rows), not distinct items | 8,523 |
| Avg Rating | Average customer `Rating` | ~3.97 |

## Visuals

| Visual | Type | Purpose |
|---|---|---|
| Total Sales by Fat Content | Donut | Impact of fat content on sales |
| Total Sales by Item Type | Bar | Item type performance |
| Fat Content by Outlet Location | Clustered bar | Low Fat vs Regular sales per location tier |
| Total Sales by Outlet Establishment Year | Area / line | Outlet age vs sales |
| Sales by Outlet Size | Donut | Outlet size contribution |
| Sales by Outlet Location | Funnel | Distribution across Tier 1 / 2 / 3 |
| All Metrics by Outlet Type | Table with conditional formatting | Side-by-side view of every KPI per outlet type |

## Interactivity

- **Slicers:** Outlet Location Type, Outlet Size, Item Type.
- **Metric selector:** a field parameter (`Metrics` table) that switches the chart section between Total Sales, Avg Sales, No of Items, and Avg Rating without duplicating visuals.
- **Reset button:** a bookmark-based button that returns slicers, visual selections, and filters to their default state.
- **Cross-filtering** between visuals.

## Dataset

`BlinkIT Grocery Data.xlsx` has 8,523 rows and 12 columns.

| Column | Description |
|---|---|
| Item Identifier | Unique item ID |
| Item Type | One of 16 categories (e.g. Dairy, Snack Foods) |
| Item Fat Content | Low Fat / Regular (after cleaning) |
| Item Visibility | Share of display area given to the item |
| Item Weight | Item weight |
| Outlet Identifier | One of 10 outlets |
| Outlet Establishment Year | 2011 to 2022 |
| Outlet Size | Small / Medium / High |
| Outlet Location Type | Tier 1 / 2 / 3 |
| Outlet Type | Grocery Store, Supermarket Type 1 / 2 / 3 |
| Sales | Sales value per record |
| Rating | Customer rating (1 to 5) |

## Data Preparation

Done in Power Query:

1. Loaded the Excel sheet and promoted headers.
2. Set data types. `Rating` must be a **decimal number**. A whole-number type rounds each rating before averaging and gives 3.9 instead of 3.97.
3. Standardised `Item Fat Content`: `LF` and `low fat` become `Low Fat`, and `reg` becomes `Regular`. This fixed 545 inconsistent rows.

The same logic was cross-checked in T-SQL (SQL Server) for each KPI and chart.

## DAX Measures

```DAX
Total Sales = SUM ( 'BlinkIT Grocery Data'[Sales] )

Avg Sales   = AVERAGE ( 'BlinkIT Grocery Data'[Sales] )

No of Items = COUNTROWS ( 'BlinkIT Grocery Data' )

Avg Rating  = AVERAGE ( 'BlinkIT Grocery Data'[Rating] )
```

The `Metrics` field-parameter table:

```DAX
Metrics = {
    ("Total Sales", NAMEOF ( 'BlinkIT Grocery Data'[Total Sales] ), 0),
    ("Avg Sales",   NAMEOF ( 'BlinkIT Grocery Data'[Avg Sales] ),   1),
    ("No of Items", NAMEOF ( 'BlinkIT Grocery Data'[No of Items] ), 2),
    ("Avg Rating",  NAMEOF ( 'BlinkIT Grocery Data'[Avg Rating] ),  3)
}
```

## Key Insights

- **Low Fat items drive about 65% of sales** ($776K vs $425K for Regular), mainly because they make up more of the catalogue (5,517 vs 3,006 records).
- **Fruits and Vegetables ($178K) and Snack Foods ($175K)** are the top item types. Seafood ($9K) and Breakfast ($16K) are the smallest.
- **Medium outlets contribute the most (42%)**, followed by Small (37%) and High (21%).
- **Tier 3 locations lead (39%)**, ahead of Tier 2 (33%) and Tier 1 (28%).
- **Supermarket Type 1 generates about 66% of sales** from 5,577 records. Grocery Stores add 13%.
- **Average sales per record is flat (about $140 to $142) in every segment.** Differences in total sales come from how many items an outlet sells, not from the value of each sale.
- **Outlets established in 2018 stand out** at $205K, because they hold 1,463 records versus roughly 930 for most other years.

## Tech Stack

Power BI Desktop, DAX, Power Query (M), T-SQL (SQL Server), Excel

## Repository Structure

```
blinkit-sales-dashboard/
├── README.md
├── Blinkit_Sales_Dashboard.pbix
├── data/
│   └── BlinkIT_Grocery_Data.xlsx
├── sql/
│   └── blinkit_analysis.sql
└── images/
    └── dashboard.png
```

## How to Run

1. Clone the repo and open `Blinkit_Sales_Dashboard.pbix` in Power BI Desktop.
2. The report points to a local file path. Go to **Home > Transform data > Data source settings > Change Source** and select `data/BlinkIT_Grocery_Data.xlsx` from your clone.
3. Click **Refresh**.

## Limitations

- `Item Weight` has 1,463 missing values (about 17%). It is not used in any KPI or visual.
- `Item Visibility` contains zeros, which look like placeholder values rather than real readings.
- "No of Items" counts records, not distinct products.
- The data has no date or customer dimension, so there is no time-series or customer-level analysis beyond establishment year.

## Credits

Business requirements and dataset are based on the Blinkit Analysis tutorial by **Data Tutorials**. Dashboard layout, the field-parameter metric selector, the reset button, and the insights above are my own work.

## Author

**Arman Shikalgar**
[GitHub](https://github.com/your-username) | [LinkedIn](https://linkedin.com/in/your-profile)
