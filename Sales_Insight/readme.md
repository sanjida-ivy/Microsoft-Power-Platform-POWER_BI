# Sales Database Workflow

## Database Overview
This project involves working with the **Sales** database containing the following tables:

- `customers`
- `date`
- `markets`
- `products`
- `transactions`

![Database Schema](path/to/database_schema_image.png)

## Data Exploration with SQL (MySQL Workbench)

### Inspecting Table Structures and Relationships
Before running queries, the table structures and their relationships were inspected to understand the schema.

![Table Relationships](path/to/table_relationships_image.png)

### Running Queries to Analyze and Understand the Data
Below are the key queries used to explore and analyze the data:

#### a. Total Sales by Market and Product Category
This query calculates the total sales for each product category within different markets:

```sql
SELECT 
    m.markets_name, 
    p.product_type, 
    SUM(t.sales_amount) AS total_sales
FROM 
    transactions t
JOIN 
    products p ON t.product_code = p.product_code
JOIN 
    markets m ON t.market_code = m.markets_code
GROUP BY 
    m.markets_name, p.product_type
ORDER BY 
    total_sales DESC;

```

---

#### b. Top 5 Customers by Total Transactions
This query identifies the top 5 customers based on the number of transactions and total sales amount:

```sql
SELECT 
    c.customer_name, 
    COUNT(t.product_code) AS total_transactions, 
    SUM(t.sales_amount) AS total_spent
FROM 
    transactions t
JOIN 
    customers c ON t.customer_code = c.customer_code
GROUP BY 
    c.customer_name
ORDER BY 
    total_spent DESC
LIMIT 5;

```

---

#### c. Monthly Sales Trend Analysis
This query analyzes sales trends on a monthly basis:

```sql
SELECT 
    d.year, 
    d.month_name, 
    SUM(t.sales_amount) AS monthly_sales
FROM 
    transactions t
JOIN 
    date d ON t.order_date = d.date
GROUP BY 
    d.year, d.month_name
ORDER BY 
    d.year, d.month_name;
```

---

#### d. Identifying Inactive Customers (No Transactions in the Last 6 Months)
This query identifies customers who haven't made transactions in the last 6 months:

```sql
SELECT 
    c.customer_code, 
    c.customer_name
FROM 
    customers c
LEFT JOIN 
    transactions t ON c.customer_code = t.customer_code
JOIN 
    date d ON t.order_date = d.date
WHERE 
    t.product_code IS NULL 
    OR d.date < DATE_SUB(CURDATE(), INTERVAL 6 MONTH);
```

---

#### e. Product Performance Across Markets
This query evaluates product performance across markets, filtering for products generating revenue greater than 5000:

```sql
SELECT 
    p.product_type, 
    m.markets_name, 
    COUNT(t.product_code) AS total_sales_count, 
    SUM(t.sales_amount) AS total_revenue
FROM 
    transactions t
JOIN 
    products p ON t.product_code = p.product_code
JOIN 
    markets m ON t.market_code = m.markets_code
GROUP BY 
    p.product_type, m.markets_name
HAVING 
    total_revenue > 5000
ORDER BY 
    total_revenue DESC;
```

---

## Connect Database with Power BI
Select 'Get Data from Another Source', choose MySQL Database, and enter the login credentials to connect.

## Data cleaning and ETL
In Power BI, data cleaning and ETL (Extract, Transform, Load), also known as data munging or data wrangling, are carried out. The process includes handling invalid values, such as removing rows with null entries in the "Zone" column and filtering out sales transactions with sales amounts of 0 or -1. Currency normalization is also applied by converting all values to INR. These steps complete the ETL process for the dataset.
## Power BI Dashboard
In addition to MySQL data exploration, a Power BI dashboard was developed to provide a dynamic and visual representation of sales data. Key features of the dashboard include:

Dynamic Revenue Display: Total revenue updates based on selected years and companies.
Sales Quantity Metrics: Sales quantity visualized by customer name.
Revenue Trends: A line chart showcasing revenue over time.
Top Performers: Insights into the top 10 companies and products, grouped by zone.
This interactive dashboard enhances data accessibility and allows stakeholders to quickly identify trends and opportunities.

![Sales Insights Summary](../Sales_Insight/Dashboard.png)

---

### Tools Used
- **Database Management System:** MySQL
- **SQL Query Interface:** MySQL Workbench
- **Data Visualization Tool:** Power BI
- **ETL and Data Cleaning:** Power BI
 

---
