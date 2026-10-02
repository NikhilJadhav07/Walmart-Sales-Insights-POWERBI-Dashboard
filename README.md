# 🛒 Walmart Sales & Store Performance Analytics | Power BI Dashboard

![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-17%20Measures-0078D4)
![Power Query](https://img.shields.io/badge/Power%20Query-ETL-217346)
![Excel](https://img.shields.io/badge/Data-Excel%20%2F%20CSV-217346?logo=microsoftexcel&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

An interactive **Power BI sales analytics dashboard** that tracks sales, profit, orders, customers and products for a retail business, and highlights the best and worst performing categories, products, states and months.

![Dashboard Overview](images/dashboard-overview.png)

---

## 📑 Table of Contents
- [Project Overview](#-project-overview)
- [Business Questions Answered](#-business-questions-answered)
- [Dashboard Features](#-dashboard-features)
- [Dataset](#-dataset)
- [Tech Stack](#-tech-stack)
- [Data Preparation](#-data-preparation)
- [DAX Measures](#-dax-measures)
- [Key Insights](#-key-insights)
- [Recommendations](#-recommendations)
- [How to Open the Dashboard](#-how-to-open-the-dashboard)
- [Repository Structure](#-repository-structure)
- [About the Author](#-about-the-author)
- [Contact](#-contact)

---

## 📌 Project Overview

Retail managers need one place to see how the business is doing and where money is being made or lost. This project converts **3,203 order lines (1,611 orders) from January 2011 to December 2014** into a single-page dashboard with KPI cards, trend charts, rankings and auto-generated text insights. Everything responds to five slicers: **Year, Month, Category, City and State**.

## ❓ Business Questions Answered

1. How much did we sell and earn, and what is the overall profit margin?
2. How are sales and profit trending month by month?
3. Which categories and products drive sales, and which ones actually drive profit?
4. Which states and customers matter most, and where are we losing money?
5. When are the busiest months for orders?
6. How does performance compare with the previous year?

## ✨ Dashboard Features

| Section | Description |
|---------|-------------|
| **Slicers** | Year, Month, Category, City, State |
| **KPI cards** | Total Sales, Total Profit, Total Orders, Total Quantity, Total Customers, Total Products |
| **Sales by Top 10 Products** | Ranked bar chart of best-selling products |
| **Sales & Profit Trend** | Monthly sales vs profit comparison |
| **Profit by Category** | Which categories earn the most profit |
| **Sales by Category** | Which categories bring the most revenue |
| **Order Count by Month** | Seasonality of order volume |
| **Live Insights panel** | Text insights written with DAX that **update with every filter** (top category, top product, top state, top customer) |

## 📊 Dataset

- **File:** [`data/walmart_sales_orders.csv`](data/walmart_sales_orders.csv)
- **Size:** 3,203 rows × 12 columns, no missing values or duplicate rows
- **Period:** 7 Jan 2011 to 31 Dec 2014
- **Coverage:** 1 country (United States), 11 states, 169 cities, 17 product categories, 1,494 products, 686 customers
- **Currency:** US dollars

| Column | Description | Type |
|--------|-------------|------|
| `Order ID` | Unique order identifier | Text |
| `Order Date` | Date the order was placed | Date |
| `Ship Date` | Date the order was shipped | Date |
| `Customer Name` | Customer who placed the order | Text |
| `Country` | Country (United States) | Text |
| `City` | City of delivery | Text |
| `State` | State of delivery | Text |
| `Category` | Product category (e.g. Chairs, Phones, Binders) | Text |
| `Product Name` | Product purchased | Text |
| `Sales` | Sales amount ($) | Decimal |
| `Quantity` | Units sold | Whole number |
| `Profit` | Profit on the line ($, can be negative) | Decimal |

## 🛠️ Tech Stack

| Tool | Used for |
|------|----------|
| **Power BI Desktop** | Data model, visuals, slicers, report design |
| **DAX** | KPI, growth, average and dynamic-text measures |
| **Power Query (M)** | Loading the Excel sheet, promoting headers, setting data types |
| **Excel / CSV** | Source data |

## 🧹 Data Preparation

Done in Power Query:

1. Connected to the Excel workbook (`Walmart` sheet).
2. Promoted the first row to headers.
3. Set data types: dates for `Order Date` and `Ship Date`, text for IDs, names and locations, decimal for `Sales` and `Profit`, whole number for `Quantity`.
4. Built a **calendar table (`DateTable`)** in DAX from the minimum to maximum order date, with Year, Month Number, Month, Year-Month and Quarter columns, to support time-intelligence measures.

## 🧮 DAX Measures

| Category | Measures |
|----------|----------|
| **Core KPIs** | Total Sales, Total Profit, Total Quantity, Total Orders, Total Customers, Total Products, Total Categories |
| **Ratios & averages** | Profit Margin, Average Order Value, Average Order Profit |
| **Time intelligence** | Previous Year Sales, Sales Growth %, Previous Year Profit, Profit Growth % (using `SAMEPERIODLASTYEAR`) |
| **Dynamic insights** | Live Insight 1, Live Insight 2, Live Insight 3 (text built with `TOPN`, `CONCATENATEX` and `FORMAT`) |

```DAX
Profit Margin = DIVIDE ( [Total Profit], [Total Sales], 0 )

Sales Growth % =
DIVIDE ( [Total Sales] - [Previous Year Sales], [Previous Year Sales], 0 )

Previous Year Sales =
CALCULATE ( [Total Sales], SAMEPERIODLASTYEAR ( DateTable[Date] ) )
```

## 💡 Key Insights

*(Figures calculated from the dataset in this repository.)*

**Overall:** Sales of **$725.5K**, profit of **$108.4K**, a **14.9% profit margin**, and an average order value of **$450**.

**Growth by year**

| Year | Sales | Profit | Margin |
|------|------:|-------:|-------:|
| 2011 | $147.9K | $20.1K | 13.6% |
| 2012 | $140.0K | $20.5K | 14.6% |
| 2013 | $187.0K | $24.0K | 12.8% |
| 2014 | $250.6K | $43.9K | 17.5% |

- Sales dipped about 5% in 2012, then grew about **34% in both 2013 and 2014**.
- 2014 was the best year: profit grew about **83%** and the margin reached **17.5%**.

**Categories and products**
- **Chairs** ($101.8K), **Phones** ($98.7K) and **Tables** ($84.8K) lead on sales, but their margins are low (**4.0%, 9.2% and 1.7%**). Higher sales do not mean higher profit.
- **Copiers** ($19.3K), **Accessories** ($16.5K), **Binders** ($16.1K) and **Paper** ($12.1K) are the top profit makers, with margins of 27% to 45%.
- **Bookcases** and **Machines** lose money overall (-$1.6K and -$0.6K).
- Top product by sales: **Canon imageCLASS 2200 Advanced Copier** ($14.0K).
- About **10% of order lines (318 of 3,203) were sold at a loss**.

**Geography and customers**
- **California generates 63% of sales** ($457.7K), followed by Washington (19%).
- **Colorado (-$6.5K), Arizona (-$3.4K) and Oregon (-$1.2K)** are loss-making states.
- Top customer by sales: Raymond Buch ($14.3K).

**Seasonality**
- Sales and orders climb through the year: **December is the peak month** ($115.9K sales) with November and December both at 234 orders; **February is the weakest** ($16.3K).

## ✅ Recommendations

1. **Review pricing and discounting on Tables, Bookcases and Machines**, which sell well but earn little or lose money.
2. **Look into the loss-making states** (Colorado, Arizona, Oregon) for shipping cost, discount or product-mix problems.
3. **Prioritise high-margin lines** such as Copiers, Accessories, Binders and Paper in promotions.
4. **Plan stock and staffing for September to December**, when order volume is highest.
5. **Reduce dependence on California** by growing other states profitably.

## ▶️ How to Open the Dashboard

1. Install [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free, Windows).
2. Clone or download this repository:
   ```bash
   git clone https://github.com/NikhilJadhav07/walmart-sales-performance-analytics-powerbi.git
   ```
3. Open `dashboard/Walmart_Sales_Dashboard.pbix`. The data is stored inside the file, so it opens directly.
4. To refresh the data from your own copy, go to **Home → Transform data → Data source settings → Change Source** and select your file.

## 📁 Repository Structure

```
walmart-sales-performance-analytics-powerbi/
├── dashboard/
│   └── Walmart_Sales_Dashboard.pbix
├── data/
│   └── walmart_sales_orders.csv
├── images/
│   └── dashboard-overview.png
├── README.md
├── LICENSE
└── .gitignore
```

## 👤 About the Author

**Nikhil Jadhav**: B.E. in Artificial Intelligence & Data Science (2026), Guru Gobind Singh College of Engineering and Research Center, Nashik. Currently a **Data Analyst Intern at Code B, Nashik** (since July 2026), and looking for software and data roles.

## 📬 Contact

| | |
|---|---|
| 📧 Email | [nikhiljadhav42135@gmail.com](mailto:nikhiljadhav42135@gmail.com) |
| 📱 Phone | +91 7588404338 |
| 💼 LinkedIn | [linkedin.com/in/nikhil-jadhav-347520423](https://www.linkedin.com/in/nikhil-jadhav-347520423) |
| 🐙 GitHub | [github.com/NikhilJadhav07](https://github.com/NikhilJadhav07) |

---

⭐ If you found this project useful, consider giving it a star.

*Disclaimer: This project is for learning and portfolio purposes only. The dataset is a sample retail dataset, not official Walmart data.*
