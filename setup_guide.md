# ⚙️ Setup Guide — Building the Power BI Dashboard

Follow these steps to build the full dashboard in Power BI Desktop.

---

## Step 1: Load the Data

1. Open **Power BI Desktop**
2. Click **Home → Get Data → Text/CSV**
3. Load `data/sales_data.csv` → Click **Load**
4. Repeat for `data/region_targets.csv`

---

## Step 2: Create the Calendar Table

1. Go to **Modeling → New Table**
2. Paste this formula:
```dax
Calendar = CALENDAR(DATE(2023,1,1), DATE(2024,12,31))
```
3. Add columns via **New Column**:
```dax
Year    = YEAR('Calendar'[Date])
Month   = FORMAT('Calendar'[Date], "MMM YYYY")
MonthNo = MONTH('Calendar'[Date])
Quarter = "Q" & QUARTER('Calendar'[Date])
Week    = WEEKNUM('Calendar'[Date])
```
4. Right-click Calendar table → **Mark as Date Table** → select `Date` column

---

## Step 3: Set Up Relationships

1. Go to **Model View** (left sidebar)
2. Create relationship: `sales_data[Order_Date]` → `Calendar[Date]` (Many-to-One)
3. Create relationship: `sales_data[Region]` → `region_targets[Region]` (Many-to-Many, or use a bridge)

> Tip: For Region+Category matching, create a calculated column:
> ```dax
> RegionCategory = sales_data[Region] & "|" & sales_data[Category]
> ```
> Do the same in region_targets table, then relate on that column.

---

## Step 4: Add All DAX Measures

1. Select the `sales_data` table in Fields pane
2. Click **Modeling → New Measure**
3. Copy each measure from `dax_measures.md` and save

---

## Step 5: Build Page 1 — Executive Summary

| Visual | Type | Fields |
|---|---|---|
| Total Sales | Card | [Total Sales] |
| Total Profit | Card | [Total Profit] |
| Profit Margin % | Card | [Profit Margin %] |
| Total Orders | Card | [Total Orders] |
| Monthly Sales Trend | Line Chart | X: Month, Y: Total Sales |
| Sales vs Target | Gauge | Value: Total Sales, Max: Total Target |

- Add **Year** and **Region** slicers at the top

---

## Step 6: Build Page 2 — Regional Analysis

| Visual | Type | Fields |
|---|---|---|
| Sales by City | Map | Location: City, Size: Total Sales |
| Sales & Profit by Region | Clustered Bar | Axis: Region, Values: Sales + Profit |
| Region Order Share | Donut Chart | Legend: Region, Values: Total Orders |
| Region Target Gap | Bar Chart | Axis: Region, Values: Target Gap |

---

## Step 7: Build Page 3 — Product Performance

| Visual | Type | Fields |
|---|---|---|
| Sales by Category | Treemap | Category: Category, Values: Total Sales |
| Category Matrix | Matrix | Rows: Category, Cols: Sub_Category, Values: Sales |
| Top 10 Products | Bar Chart | Axis: Product_Name, Values: Total Sales (filter Top 10) |

---

## Step 8: Build Page 4 — Customer & Segment Analysis

| Visual | Type | Fields |
|---|---|---|
| Sales by Segment | Pie Chart | Legend: Segment, Values: Total Sales |
| Top 10 Customers | Bar Chart | Axis: Customer_Name, Values: Top 10 Customers |
| Profit by Segment | Funnel | Category: Segment, Values: Total Profit |

---

## Step 9: Build Page 5 — Time Intelligence

| Visual | Type | Fields |
|---|---|---|
| YTD Sales | Line Chart | X: Month, Y: YTD Sales |
| MoM Growth | Waterfall | Category: Month, Values: MoM Sales Growth % |
| YoY Comparison | Clustered Column | X: Month, Y: Total Sales + Prev Year Sales |

---

## Step 10: Format & Polish

- Apply a consistent theme: **View → Themes → Executive** or **Innovation**
- Add a company/project title text box on each page
- Add your **Name and Roll Number** in the footer of each page (Insert → Text Box)
- Use **Format → Page Background** for a clean dark/light background
- Add **Bookmarks** for each page for navigation buttons

---

## Step 11: Export & Submit

1. **Save** the file as `RetailSalesDashboard.pbix`
2. Take screenshots of each page and save them in `/screenshots/`
3. Place the `.pbix` file in the project root folder
4. Zip everything → `RetailSalesDashboard.zip`
5. Upload to GitHub and submit the link

---

## ✅ Final Checklist

- [ ] sales_data.csv loaded (500 rows)
- [ ] region_targets.csv loaded (96 rows)
- [ ] Calendar table created and marked as date table
- [ ] All relationships set up in Model View
- [ ] All DAX measures added
- [ ] 5 dashboard pages built
- [ ] Slicers for Year, Quarter, Region, Category added
- [ ] Name and Roll Number visible on dashboard
- [ ] .pbix file saved
- [ ] Screenshots taken
- [ ] Documentation PDF ready
- [ ] Uploaded to GitHub
