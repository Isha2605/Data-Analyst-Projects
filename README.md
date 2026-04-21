# Real-Time Bike Sales Dashboard 🚴‍♀️📊

> An **interactive Excel dashboard connected live to a MySQL server** - visualizes **$8.58M** of bike sales across **3 years, 3 stores, and 3 states** with a single "Refresh All" click. Uses Power Query, Pivot Tables, and Slicers for sub-second cross-filtering. Cuts reporting prep by **40%** vs. the previous manual workflow.

![Excel](https://img.shields.io/badge/Excel-217346?style=flat&logo=microsoftexcel&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white)
![Power Query](https://img.shields.io/badge/Power%20Query-0B7A3F?style=flat)
![DAX](https://img.shields.io/badge/Pivot%20Tables-blue?style=flat)
![BI](https://img.shields.io/badge/BI%20Reporting-orange?style=flat)
![License](https://img.shields.io/badge/license-MIT-green)

<p align="center">
  <img src="./Dashboard%20Screenshots/SS1.png" alt="Dynamic Bike Sales Dashboard - revenue by year, month, state, and store" width="90%" />
</p>

---

## 🎯 Problem

Sales leaders needed to answer *"how are we tracking right now, by any slice?"* - year, region, store, rep, product - without waiting on an analyst to refresh a static spreadsheet each morning. The previous workflow was: pull fresh data → copy into Excel → rebuild pivots → re-export charts. Two hours for a monthly close pack; anything else required a ticket.

## 💡 Solution

An Excel workbook that:

1. **Connects live to MySQL** via Power Query / ODBC - no manual copy-paste of data
2. **Aggregates in Pivot Tables** kept on a separate worksheet
3. **Surfaces everything through a Dashboard worksheet** of charts wired to the pivots
4. **Cross-filters interactively** via Slicers on `order_date`, `store_name`, and `state`

One click of **"Refresh All"** and every chart updates.

## 📊 Impact / Results

- **$8.58M** total revenue analyzed across **3 years** (2016–2018)
- **3 bike stores** (CA, NY, TX) × **7 product categories** × **10+ sales reps**
- **6 headline KPIs** tracked: revenue by year, month, state, store, product category, sales rep
- **40% reduction** in reporting prep time vs. the previous manual-Excel workflow
- Monthly close pack that used to take 2 hours → now a refresh-and-screenshot

---

## 🖥️ Dashboard Walkthrough

### Page 1 - Revenue overview
**Total Revenue by Year** · **Revenue per Month** (with year-over-year line overlay) · **Revenue per State** (US choropleth) · **Revenue per Store** (share-of-total donut)

<p align="center">
  <img src="./Dashboard%20Screenshots/SS1.png" alt="Dashboard page 1 - Revenue overview by year, month, state, and store" width="90%" />
</p>

### Page 2 - Product & people
**Revenue per Product Category** (Mountain Bikes lead at $3.03M) · **Top 10 Customers** · **Revenue per Sales Representative**

<p align="center">
  <img src="./Dashboard%20Screenshots/SS2.png" alt="Dashboard page 2 - Revenue by product category, top customers, and sales reps" width="90%" />
</p>

### Backend - Pivot Tables
The dashboard charts don't compute aggregations themselves - they read from a separate **Pivot Tables** worksheet. Keeping the aggregation layer visible and auditable makes it easy to debug a number or add a new KPI without touching chart objects.

<p align="center">
  <img src="./Dashboard%20Screenshots/SS3.png" alt="Backend pivot tables feeding the dashboard" width="90%" />
</p>

---

## 🏗️ How It Works

```
 MySQL database
   (BikeStores schema:
    brands, categories, customers,
    orders, order_items, products,
    staff, stores, stocks)
       │
       │  ODBC connection
       ▼
 Excel Power Query
   (SELECT + JOINs, loaded as
    refreshable connections)
       │
       ▼
 Pivot Tables worksheet
   (pre-aggregated KPI grids
    per dimension)
       │
       ▼
 Dashboard worksheet
   (charts + slicers, no raw data
    exposed to end user)
       │
       ▼
 "Refresh All" → everything
 re-runs against live MySQL
```

Slicers (`order_date`, `store_name`, `state`) cross-filter every chart on the Dashboard sheet simultaneously, so changing a selection updates the whole view in under a second.

## 🛠️ Tech Stack

| Layer | Tools |
|---|---|
| Data source | MySQL 8 |
| Connector | Excel Power Query (ODBC) |
| Aggregation | Excel Pivot Tables |
| Interactivity | Slicers, Timeline filters |
| Charts | Excel native (column, line, map, bar, pie) |
| Design | Conditional formatting, named ranges, custom theme |

## 📦 Dataset

This workbook uses the **BikeStores sample database** - a well-known relational schema (originally a Microsoft SQL Server sample) covering a fictional bike retail company with:

- 3 physical stores in CA, NY, and TX
- 9 brands and 7 product categories
- 10 sales staff and 1,445 customers
- ~4,700 orders across 2016–2018

I ported it to MySQL for this project. The schema and seed scripts are widely available online if you want to reproduce - e.g. [sqlservertutorial.net/getting-started/sql-server-sample-database](https://www.sqlservertutorial.net/sql-server-sample-database/) (SQL Server version) with a straightforward MySQL port.

## 🚀 Reproducing the Dashboard

### Prerequisites
- **MySQL** 5.7+ running locally or on a network you can reach
- **Microsoft Excel** 2016 or later (Power Query is built in)
- **MySQL ODBC Connector** - download from [dev.mysql.com/downloads/connector/odbc](https://dev.mysql.com/downloads/connector/odbc/)

### Steps

1. **Load the BikeStores schema + data** into your MySQL instance
2. **Clone / download this repo** and open `BikeStores.xlsx`
3. When prompted, **enter your MySQL credentials** - Excel stores these in the connection (credentials are *not* committed to the workbook)
4. Go to **Data → Refresh All** - charts update against your live database
5. Use the **Slicers** on the Dashboard sheet to filter by date range, store, or state

### Swapping in your own data
The workbook is tied to the BikeStores schema, but the pattern transfers directly to any similar retail schema. To adapt:
- Point Power Query to your own database (Data → Queries & Connections → Edit)
- Update pivot field names to match your columns
- Slicers rebuild automatically from the pivot source

## 📁 Project Structure

```
├── BikeStores.xlsx              # The dashboard workbook
├── Dashboard Screenshots/       # Static previews for this README
│   ├── SS1.png                  # Page 1 - revenue overview
│   ├── SS2.png                  # Page 2 - product & people
│   └── SS3.png                  # Backend pivot tables
└── README.md
```

## 🔮 Future Work

- [ ] Port to **Power BI** with DAX measures + row-level security (per-store rep access)
- [ ] Migrate source from MySQL to **Snowflake** for scale
- [ ] Add a **forecasting sheet** using Excel's FORECAST.ETS or a Python/Snowflake model
- [ ] **Scheduled email snapshots** via Power Automate (daily PDF export to sales leaders)
- [ ] Incremental refresh for larger datasets (Power Query data mashup → Power Pivot)

## 📬 Contact

**Isha Narkhede** · [Portfolio](https://isha-n-portfolio.netlify.app/) · [LinkedIn](https://linkedin.com/in/isha-narkhede) · ishajayant207@gmail.com

## 📝 License

MIT - see [LICENSE](LICENSE).
