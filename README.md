

<div align="center">

# 📊 Classic Model Sales Analytics — Power BI with Snowflake

<img src="https://img.shields.io/badge/Power%20BI-Executive%20Dashboard-F2C811?style=for-the-badge&logo=powerbi&logoColor=black"/>
<img src="https://img.shields.io/badge/Snowflake-Warhouse-orange?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Advanced-DAX-blue?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Power%20Query-Data%20Transformation-success?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Data%20Model-Star%20Schema-red?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge"/>

---

### 🚀 An Interactive  Dashboard for Sales, Profitability, Customer Behavior, Product Performance & Business Intelligence

</div>

An end-to-end business intelligence project built on top of the classic **ClassicModels** sales dataset, sourced live from **Snowflake** and modeled in **Power BI Desktop**. The project covers sales, margin & profit trends, order-status operations, customer behavior, and product performance — with 60+ DAX measures answering real business questions a sales or operations team would actually ask.

---
# 🖼 Dashboard Preview


## 📍 Dashboard (Page 1)
![Executive Dashboard](https://github.com/saif146/classicmodels-snowflake-powerbi/blob/main/dashboard%20image/financial.png)
## 📍 Dashboard (Page 2)
![Executive Dashboard](https://github.com/saif146/classicmodels-snowflake-powerbi/blob/main/dashboard%20image/product.png)
## 📍 Dashboard (Page 3)
![Executive Dashboard](https://github.com/saif146/classicmodels-snowflake-powerbi/blob/main/dashboard%20image/customer.png)

## 🔗 Data Source

Data is pulled directly from a **Snowflake** warehouse (not a static CSV/Excel dump), using Power Query (M) in **Import mode**:

- **Warehouse:** Snowflake (`COMPUTE_WH`)
- **Database / Schema:** `CLASSIC_DATABASE.CLASSIC_SCHEMA`
- **Tables ingested:** `ORDERS`, `ORDERDETAILS`, `PRODUCTS`, `CUSTOMERS`, `EMPLOYEES`, `OFFICES`, `PAYMENTS`
- Additional columns (`MARGIN`, `PROFIT`) are engineered in Power Query by joining `order details` back to `products` on cost price.

## 🧱 Data Model

A star-schema-style model with a dedicated calendar table and clean fact/dimension separation:

```
DateTable ──< orders ──< order details >── products
                │
                ├──< payments
                └──< customers
                         
employees >── offices
```
![Data Modeling](https://github.com/saif146/classicmodels-snowflake-powerbi/blob/main/dashboard%20image/erd.png)

- **Fact tables:** `orders`, `order details`, `payments`
- **Dimension tables:** `customers`, `products`, `employees`, `offices`, `DateTable`
- All relationships are single-direction (One → Many) by default; two relationships are **intentionally bidirectional** (`orders ↔ customers`, `order details ↔ products`) after diagnosing a real filter-propagation issue where slicers weren't reaching the `products` table through a 4-table relationship chain.

## 📁 Measure Organization

Measures are grouped into dedicated tables with display folders for a clean field list in report view:

| Measure Table | Display Folders |
|---|---|
| `All Measures` | sales, Margin, profit, Quantity, Order Status, Top 3 month contribution |
| `Product Mesaures` | Product Measure |
| `Customer Measure` | Country performance |

## 📈 Key Business Questions Answered

- **Sales & Profitability:** What's total revenue, margin, and profit — and how does it compare to last month / last year?
- **Product Performance:** Which product line contributes the most to profit? What's the average product price? What are the top-selling products?
- **Order Operations:** How many orders are Placed, Shipped, On Hold, Disputed, Resolved, or Cancelled in a given period?
- **Returns Analysis:** Which 5 products account for the most cancelled-order quantity?
- **Customer Behavior:** Which customers have never placed an order? Who are the top customers by revenue? What's average revenue per customer?
- **Employee & Regional Performance:** How are employees and countries performing against each other?

## 🧮 Example DAX

A few measures that reflect real modeling decisions rather than boilerplate:

**Safe percentage-of-total contribution** (respects Year/Month filters, only removes the `productLine` filter):
```dax
revenue_by_product_line =
DIVIDE(
    [Total profit],
    CALCULATE([Total profit], ALL(products[productLine]))
)
```

**Dynamic Top-N as a single text measure** (for card visuals), with context transition per product:
```dax
Top 5 Returned Products =
VAR RankedProducts =
    TOPN(
        5,
        FILTER(
            ADDCOLUMNS(VALUES(products[productName]), "@Qty", [Cancelled Qty]),
            [@Qty] > 0
        ),
        [@Qty], DESC
    )
RETURN
    CONCATENATEX(RankedProducts, products[productName] & " (" & [@Qty] & ")", UNICHAR(10), [@Qty], DESC)
```

**Order-status breakdown**, all built as distinct-count-with-filter measures:
```dax
Orders Cancelled = CALCULATE(DISTINCTCOUNT(orders[ordernumber]), orders[status] = "Cancelled")
```

## 🛠️ Skills Demonstrated

- Connecting Power BI to a **cloud data warehouse (Snowflake)** and shaping data in Power Query
- **Star-schema data modeling** and relationship design (cardinality, cross-filter direction)
- **Diagnosing and fixing filter-propagation bugs** across multi-table relationship chains
- Intermediate–advanced **DAX**: `CALCULATE`, `ALL`, `TOPN`, `DIVIDE`, `CONCATENATEX`, context transition, time intelligence (YoY/MoM)
- Measure organization for report usability (display folders, hidden helper measures, descriptions)

## 🚧 Known Improvements (in progress)

This project is actively being refined. Current cleanup items:
- Adding descriptions to all measures for field-list clarity

# 👨‍💻 About the Developer

## MD. Saiful Islam

**Data Analyst | Power BI Developer | SQL Enthusiast**

I enjoy transforming raw business data into interactive dashboards that support strategic decision-making. My interests include Business Intelligence, Data Visualization, SQL, Power BI, and analytical problem-solving.

---

# 📬 Connect With Me

- 💼 LinkedIn: *(https://www.linkedin.com/in/saiful-islam-7b7a64268/)*
- 🐙 GitHub: *([Add your GitHub profile URL here](https://github.com/saif146))*
- 📧 Email: *(saifulislam170104146@gmail.com)*

---

# ⭐ Support

If you found this project useful or inspiring:

⭐ Star this repository

🍴 Fork it

💬 Share your feedback

Your support is greatly appreciated!

---

<div align="center">

## 🎉 Thank You for Visiting!

**If you enjoyed exploring this project, don't forget to leave a ⭐ on the repository.**

### Happy Analyzing! 📊

</div>
