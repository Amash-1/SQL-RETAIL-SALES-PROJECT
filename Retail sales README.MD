 Retail Sales Analysis SQL Project 🛒

*Beginner SQL project where I built a retail sales database, cleaned the data, and wrote queries to explore sales trends, customers, and product performance.*

---

## 📌 Project Overview

**Project Title**: Retail Sales Analysis  
**Level**: Beginner  
**Database**: `p1_retail_db`  

This project is part of my SQL learning journey. I worked with a retail sales dataset containing customer, product, and transaction details. The goal was to build the database from scratch, clean it, and then analyze it using SQL queries. By doing this, I was able to practice database design, data cleaning, exploratory data analysis, and business-focused SQL queries.

---

## 🎯 Objectives

1. **Database Setup** – Create and populate a retail sales database.  
2. **Data Cleaning** – Check and remove incomplete records.  
3. **Exploratory Data Analysis (EDA)** – Explore the dataset to understand it better.  
4. **Business Analysis** – Write SQL queries to answer real business questions.  

---

## 🗂️ Project Structure

### 1. Database Setup

Created a database called `p1_retail_db` and a table named `retail_sales` to store the dataset.

```sql
CREATE DATABASE p1_retail_db;

CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);
2. Data Exploration & Cleaning
Counted records, checked unique values, and removed incomplete rows.

sql
Copy code
-- Total number of records
SELECT COUNT(*) FROM retail_sales;

-- Unique customers
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;

-- Unique categories
SELECT DISTINCT category FROM retail_sales;

-- Check for nulls
SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

-- Remove incomplete rows
DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
3. Data Analysis & Queries
I wrote SQL queries to answer business-related questions.

🔹 Sales made on 2022-11-05
sql
Copy code
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
🔹 Clothing sales with quantity > 4 in Nov-2022
sql
Copy code
SELECT *
FROM retail_sales
WHERE category = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantity >= 4;
🔹 Total sales for each category
sql
Copy code
SELECT 
    category,
    SUM(total_sale) AS net_sale,
    COUNT(*) AS total_orders
FROM retail_sales
GROUP BY 1;
🔹 Average age of Beauty customers
sql
Copy code
SELECT
    ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
🔹 Transactions with total_sale > 1000
sql
Copy code
SELECT * FROM retail_sales
WHERE total_sale > 1000;
🔹 Number of transactions by gender in each category
sql
Copy code
SELECT 
    category,
    gender,
    COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY 1;
🔹 Best-selling month in each year
sql
Copy code
SELECT 
       year,
       month,
       avg_sale
FROM 
(    
    SELECT 
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS rank
    FROM retail_sales
    GROUP BY 1, 2
) AS t1
WHERE rank = 1;
🔹 Top 5 customers by total sales
sql
Copy code
SELECT 
    customer_id,
    SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
🔹 Unique customers per category
sql
Copy code
SELECT 
    category,    
    COUNT(DISTINCT customer_id) AS cnt_unique_cs
FROM retail_sales
GROUP BY category;
🔹 Orders by shift (Morning, Afternoon, Evening)
sql
Copy code
WITH hourly_sale AS
(
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) AS total_orders    
FROM hourly_sale
GROUP BY shift;
📊 Findings
Sales spread across multiple age groups and categories (Clothing & Beauty stand out).

Some high-value transactions exceed 1000.

Seasonal trends show certain months perform better.

A few customers contribute heavily to overall sales.

📑 Reports
Sales Summary: Total sales, categories, demographics.

Trend Analysis: Monthly and shift-based patterns.

Customer Insights: Top customers and unique buyers per category.

✅ Conclusion
This project gave me practical SQL experience — setting up a database, cleaning data, performing EDA, and answering real business questions. It highlighted useful insights into sales trends, customer behavior, and product performance.

pgsql
Copy code
