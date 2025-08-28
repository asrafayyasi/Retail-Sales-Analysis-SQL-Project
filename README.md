# 🛍️ Retail Sales Analysis SQL Project

## 📌 Project Overview

**Project Title**: **Retail Sales Analysis**  
**Database**: `p1_retail_db`

This project is designed to **demonstrate SQL skills and techniques** commonly used by data analysts to:

- 🏗️ **Set up databases**
- 🧹 **Clean messy datasets**
- 📊 **Perform exploratory analysis**
- 💡 **Generate business insights**

You’ll work with **retail transaction data** containing details like:  
🗓️ Transaction Date & Time  
👤 Customer Demographics
🎁 Product Category
🔢 Purchase Quantity
💰 Total Sales

✨ Perfect for anyone starting their **data analysis journey** and looking to build a **solid SQL foundation**.

---

## 🎯 Objectives

Through this project, you will learn to:

1. 🛠️ **Database Setup** – Create & populate a retail sales database.
2. 🧹 **Data Cleaning** – Detect and remove records with missing/null values.
3. 🔍 **Exploratory Data Analysis (EDA)** – Explore the dataset to uncover basic trends.
4. 📈 **Business Analysis** – Answer practical business questions with SQL queries to derive insights.

---

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `p1_retail_db`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

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
```

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```sql
SELECT COUNT(*) FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;

SELECT * FROM retail_sales
WHERE
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR
    gender IS NULL OR age IS NULL OR category IS NULL OR
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

DELETE FROM retail_sales
WHERE
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR
    gender IS NULL OR age IS NULL OR category IS NULL OR
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:

```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:

```sql
SELECT
  *
FROM retail_sales
WHERE
    category = 'Clothing'
    AND
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND
    quantity >= 4
```

3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:

```sql
SELECT
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail_sales
GROUP BY 1
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**:

```sql
SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty'
```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:

```sql
SELECT * FROM retail_sales
WHERE total_sale > 1000
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**:

```sql
SELECT
    category,
    gender,
    COUNT(*) as total_trans
FROM retail_sales
GROUP
    BY
    category,
    gender
ORDER BY 1
```

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:

```sql
SELECT
       year,
       month,
    avg_sale
FROM
(
SELECT
    EXTRACT(YEAR FROM sale_date) as year,
    EXTRACT(MONTH FROM sale_date) as month,
    AVG(total_sale) as avg_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
FROM retail_sales
GROUP BY 1, 2
) as t1
WHERE rank = 1
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales **:

```sql
SELECT
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5
```

9. **Write a SQL query to find the number of unique customers who purchased items from each category.**:

```sql
SELECT
    category,
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail_sales
GROUP BY category
```

10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:

```sql
WITH hourly_sale
AS
(
SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
FROM retail_sales
)
SELECT
    shift,
    COUNT(*) as total_orders
FROM hourly_sale
GROUP BY shift
```

## 🔎 Findings

✨ From the analysis, here are the **key takeaways**:

- 👨‍👩‍👧 **Customer Demographics**:  
  Customers span across **multiple age groups**, with strong engagement in categories like **Clothing** and **Beauty**.

- 💎 **High-Value Transactions**:  
  Several purchases exceeded **1000+**, signaling the presence of **premium customers**.

- 📊 **Sales Trends**:  
  Monthly breakdowns reveal **seasonal spikes** and **peak periods** that can help with sales forecasting.

- 🏆 **Customer Insights**:  
  The analysis highlights the **top-spending customers** and the **most popular categories** driving revenue.

---

## 📑 Reports

The project produces a set of **insightful reports**:

- 📌 **Sales Summary** – Overview of total sales, customer demographics, and category performance.
- 📈 **Trend Analysis** – Visualization of sales patterns across months and time-shifts (Morning, Afternoon, Evening).
- 👥 **Customer Insights** – Profiles of **top customers** and counts of **unique buyers per category**.

---

## ✅ Conclusion

This project is a **comprehensive SQL case study** that covers:

- 🏗️ **Database Setup**
- 🧹 **Data Cleaning**
- 🔍 **Exploratory Data Analysis (EDA)**
- 💡 **Business-Oriented SQL Queries**

👉 The insights empower **data-driven decision making** by revealing:

- Customer behavior
- Sales performance
- Product category dynamics

---

## 🚀 How to Use

Follow these steps to get started:

1. **Clone the Repository** 🖥️
   ```bash
   git clone https://github.com/yourusername/retail-sales-analysis.git
   ```
2. **Set Up the Database**: Run the SQL scripts provided in the `database_setup.sql` file to create and populate the database.
3. **Run the Queries**: Use the SQL queries provided in the `analysis_queries.sql` file to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.
