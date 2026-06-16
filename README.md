# 🛒 Zepto Inventory & Product Analysis using PostgreSQL

## 📌 Project Overview

This project performs **data exploration, data cleaning, and business analysis** on a Zepto product inventory dataset using **PostgreSQL**.

The goal of this project is to extract useful insights from raw inventory data such as:

* Product availability
* Discount analysis
* Revenue estimation
* Inventory management
* Category-level insights
* Product pricing optimization

---

## 🛠️ Tech Stack

* **Database:** PostgreSQL
* **Language:** SQL
* **Concepts Used:**

  * DDL (Data Definition Language)
  * DML (Data Manipulation Language)
  * Aggregate Functions
  * Data Cleaning
  * CASE Statements
  * GROUP BY
  * Sorting & Filtering
  * Business Analytics

---

# 📂 Database Schema

```sql
CREATE TABLE zepto(
    sku_id SERIAL PRIMARY KEY,
    Category VARCHAR(150) NOT NULL,
    name VARCHAR(150) NOT NULL,
    mrp NUMERIC(8,2),
    discountPercent NUMERIC(5,2),
    availableQuantity INTEGER,
    discountedSellingPrice NUMERIC(8,2),
    weightInGms INTEGER,
    outOfStock BOOLEAN,
    quantity INTEGER
);
```

---

# 🔍 Data Exploration

### Count Total Records

```sql
SELECT COUNT(*) FROM zepto;
```

### View Sample Data

```sql
SELECT * FROM zepto LIMIT 5;
```

### Find Missing Values

```sql
SELECT *
FROM zepto
WHERE name IS NULL
OR Category IS NULL
OR mrp IS NULL
OR discountedSellingPrice IS NULL
OR weightInGms IS NULL
OR availableQuantity IS NULL
OR outOfStock IS NULL
OR quantity IS NULL;
```

### Unique Categories

```sql
SELECT DISTINCT Category
FROM zepto
ORDER BY Category;
```

### Stock Availability Analysis

```sql
SELECT outOfStock, COUNT(sku_id)
FROM zepto
GROUP BY outOfStock;
```

### Duplicate Product Detection

```sql
SELECT name,
COUNT(sku_id) AS Number_of_SKUs
FROM zepto
GROUP BY name
HAVING COUNT(sku_id) > 1;
```

---

# 🧹 Data Cleaning

### Remove Invalid Pricing

```sql
DELETE FROM zepto
WHERE mrp = 0;
```

### Convert Paise → Rupees

```sql
UPDATE zepto
SET
mrp = mrp / 100,
discountedSellingPrice = discountedSellingPrice / 100;
```

---

# 📊 Business Analysis Queries

## Q1. Top 10 Best Value Products

```sql
SELECT DISTINCT name, discountedSellingPrice
FROM zepto
ORDER BY discountedSellingPrice DESC
LIMIT 10;
```

---

## Q2. High MRP Products That Are Out Of Stock

```sql
SELECT name, mrp
FROM zepto
WHERE outOfStock = TRUE
AND mrp > 300
ORDER BY mrp DESC;
```

---

## Q3. Estimated Revenue Per Category

```sql
SELECT Category,
SUM(discountedSellingPrice * availableQuantity)
AS total_revenue
FROM zepto
GROUP BY Category;
```

---

## Q4. Premium Products with Low Discount

```sql
SELECT DISTINCT
name,
mrp,
discountPercent
FROM zepto
WHERE mrp > 500
AND discountPercent < 10;
```

---

## Q5. Categories with Highest Average Discount

```sql
SELECT category,
ROUND(AVG(discountPercent),2)
AS avg_discount
FROM zepto
GROUP BY category
ORDER BY avg_discount DESC
LIMIT 5;
```

---

## Q6. Price Per Gram Analysis

```sql
SELECT
name,
weightInGms,
discountedSellingPrice,
ROUND(
discountedSellingPrice/weightInGms,
2
)
AS price_per_gram
FROM zepto
WHERE weightInGms >= 100;
```

---

## Q7. Product Weight Segmentation

```sql
SELECT
name,
weightInGms,
CASE
WHEN weightInGms < 1000 THEN 'Low'
WHEN weightInGms < 5000 THEN 'Medium'
ELSE 'Bulk'
END AS weight_category
FROM zepto;
```

---

## Q8. Total Inventory Weight Per Category

```sql
SELECT
Category,
SUM(weightInGms * availableQuantity)
AS total_weight
FROM zepto
GROUP BY Category;
```

---

# 📈 Key Insights

* Identified product categories with the highest discounts.
* Estimated category-wise revenue generation.
* Detected duplicate SKUs.
* Cleaned invalid product pricing.
* Calculated inventory weight distribution.
* Measured product value using price-per-gram analysis.

---

# 🚀 How To Run

```bash
1. Install PostgreSQL
2. Create Database
3. Import Dataset
4. Execute SQL Script
5. Run Analytical Queries
```

---

# 📌 Future Improvements

* Add dashboard using Power BI / Tableau
* Build PostgreSQL views
* Create stored procedures
* Integrate with Python analytics
* Automate reporting

---

## 👨‍💻 Author

Made with SQL & PostgreSQL for Data Analytics Practice.
