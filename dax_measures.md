# DAX Measures — Retail Sales Dashboard

Copy these measures into Power BI Desktop under **Modeling → New Measure**.

---

## 📐 Basic Aggregations

```dax
Total Sales =
SUM(sales_data[Sales])
```

```dax
Total Profit =
SUM(sales_data[Profit])
```

```dax
Total Orders =
DISTINCTCOUNT(sales_data[Order_ID])
```

```dax
Total Quantity =
SUM(sales_data[Quantity])
```

```dax
Avg Order Value =
DIVIDE([Total Sales], [Total Orders], 0)
```

---

## 📊 Profit & Margin

```dax
Profit Margin % =
DIVIDE([Total Profit], [Total Sales], 0) * 100
```

```dax
Avg Discount % =
AVERAGE(sales_data[Discount]) * 100
```

---

## 📅 Time Intelligence (requires a Date table)

> First create a Calendar table:
> ```dax
> Calendar =
> CALENDAR(DATE(2023,1,1), DATE(2024,12,31))
> ```
> Then add: `Year = YEAR('Calendar'[Date])`, `Month = FORMAT('Calendar'[Date],"MMM")`, `Quarter = "Q" & QUARTER('Calendar'[Date])`
> Mark it as a Date Table and create a relationship with `sales_data[Order_Date]`.

```dax
YTD Sales =
TOTALYTD([Total Sales], 'Calendar'[Date])
```

```dax
YTD Profit =
TOTALYTD([Total Profit], 'Calendar'[Date])
```

```dax
Prev Month Sales =
CALCULATE([Total Sales], DATEADD('Calendar'[Date], -1, MONTH))
```

```dax
MoM Sales Growth % =
DIVIDE(
    [Total Sales] - [Prev Month Sales],
    [Prev Month Sales],
    0
) * 100
```

```dax
Prev Year Sales =
CALCULATE([Total Sales], SAMEPERIODLASTYEAR('Calendar'[Date]))
```

```dax
YoY Growth % =
DIVIDE(
    [Total Sales] - [Prev Year Sales],
    [Prev Year Sales],
    0
) * 100
```

---

## 🎯 Target vs Actual

```dax
Total Target =
SUM(region_targets[Target_Sales])
```

```dax
Sales vs Target % =
DIVIDE([Total Sales], [Total Target], 0) * 100
```

```dax
Target Gap =
[Total Sales] - [Total Target]
```

---

## 🏆 Rankings

```dax
Region Rank by Sales =
RANKX(
    ALL(sales_data[Region]),
    [Total Sales],
    ,
    DESC,
    DENSE
)
```

```dax
Top 10 Customers =
IF(
    RANKX(ALL(sales_data[Customer_Name]), [Total Sales], , DESC, DENSE) <= 10,
    [Total Sales],
    BLANK()
)
```

---

## 💡 Dynamic KPI Labels

```dax
Profit Status =
IF([Profit Margin %] >= 20, "✅ Healthy",
    IF([Profit Margin %] >= 10, "⚠️ Average", "❌ Low"))
```

```dax
Sales Achievement Label =
IF([Sales vs Target %] >= 100, "Target Met ✅", "Below Target ⚠️")
```
