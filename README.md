Retail Sales Analysis Project

Overview
This project is a practical demonstration of SQL skills and techniques commonly used by data analysts. You'll explore, clean, and analyze a retail sales dataset using SQL. The goal is to set up a retail sales database, conduct exploratory data analysis (EDA), and answer business questions with meaningful SQL queries. This project is perfect for beginners looking to strengthen their SQL skills and develop a solid understanding of data analysis.

Objectives
Database Setup: Create and populate a retail sales database.
Data Cleaning: Identify and remove records with missing or null values.
Exploratory Data Analysis: Perform basic EDA to understand the data better.
Business Analysis: Use SQL to generate insights and answer key business questions.
Project Structure
1. Database Setup
Steps:

Create a new database named p1_retail_db.
Set up a table called retail_sales to store sales data. The table includes columns like transaction ID, sale date, sale time, customer details, product category, quantity, pricing, and sales amount.
SQL Code to Create the Database and Table:

sql
Copy code
CREATE DATABASE p1_retail_db;

CREATE TABLE retail_sales (
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
2. Data Cleaning & Exploration
Perform basic data checks and clean up any inconsistencies:

Total Records: Check the total number of records.
Unique Customers: Count distinct customers.
Product Categories: Identify all unique product categories.
Null Values: Check for null values and delete those records.
SQL Queries:

sql
Copy code
-- Record Count
SELECT COUNT(*) FROM retail_sales;

-- Unique Customer Count
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;

-- List Unique Categories
SELECT DISTINCT category FROM retail_sales;

-- Check for Null Values
SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

-- Delete Records with Null Values
DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
3. Data Analysis & Insights
Here are some key SQL queries used to gain insights from the data:

Sales on Specific Date:

sql
Copy code
SELECT * FROM retail_sales WHERE sale_date = '2022-11-05';
Clothing Sales in November 2022 with Quantity > 4:

sql
Copy code
SELECT * FROM retail_sales
WHERE category = 'Clothing'
AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
AND quantity > 4;
Total Sales by Category:

sql
Copy code
SELECT category, SUM(total_sale) AS net_sale, COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;
Average Age for Beauty Category Purchases:

sql
Copy code
SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
High-Value Transactions (> 1000):

sql
Copy code
SELECT * FROM retail_sales WHERE total_sale > 1000;
Transactions by Gender and Category:

sql
Copy code
SELECT category, gender, COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category;
Average Monthly Sales & Best Selling Month Each Year:

sql
Copy code
SELECT year, month, avg_sale
FROM (
    SELECT 
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER (PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS rank
    FROM retail_sales
    GROUP BY year, month
) AS ranked_sales
WHERE rank = 1;
Top 5 Customers by Total Sales:

sql
Copy code
SELECT customer_id, SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
Unique Customers by Category:

sql
Copy code
SELECT category, COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
GROUP BY category;
Orders by Time of Day (Shifts):

sql
Copy code
WITH hourly_sale AS (
    SELECT *, 
           CASE 
               WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
               WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
               ELSE 'Evening'
           END AS shift
    FROM retail_sales
)
SELECT shift, COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;
Findings
Customer Demographics: The data includes diverse age groups with purchases spanning categories like Clothing and Beauty.
High-Value Transactions: Notable number of sales above 1000, reflecting high-value purchases.
Sales Trends: Monthly sales analysis reveals peak and slow seasons.
Customer Insights: Identification of top-spending customers and the most popular product categories.
Reports
Sales Summary: Detailed overview of sales performance, customer demographics, and category insights.
Trend Analysis: Highlights monthly sales patterns and peak sales periods.
Customer Insights: Reports on top customers and the number of unique shoppers per category.
Conclusion
This project is a comprehensive beginner-friendly guide to using SQL for data analysis. It covers database creation, data cleaning, exploratory analysis, and strategic queries to derive business insights. These findings can inform better business decisions by understanding sales dynamics, customer behavior, and product performance.

How to Use This Project
Clone the Repository: Download the project from GitHub.
Set Up the Database: Execute the provided SQL scripts to create and populate the database.
Run the Queries: Use the SQL queries provided to analyze the data.
Customize and Explore: Modify the queries as needed to gain additional insights.
