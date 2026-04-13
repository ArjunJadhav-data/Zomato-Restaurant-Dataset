# 🍽️ Zomato Data Analysis: End-to-End BI Solution
<p align="center">
  <a href="https://www.freelogovectors.net/zomato-logo-2/">
    <img src="https://www.freelogovectors.net/wp-content/uploads/2024/03/zomato-logo-freelogovectors.net_.png" alt="Zomato Logo" width="400"/>
  </a>
</p>

# 🍽️ Zomato Data Analysis: End-to-End BI Solution
[![SQL](https://img.shields.io/badge/SQL-Data_Cleaning-blue?style=for-the-badge&logo=postgresql)](https://github.com/)  
[![Power BI](https://img.shields.io/badge/Power_BI-DAX_Modeling-yellow?style=for-the-badge&logo=powerbi)](https://github.com/)  
[![Excel](https://img.shields.io/badge/Excel-Data_Modeling-217346?style=for-the-badge&logo=microsoftexcel)](https://github.com/)  
[![Tableau](https://img.shields.io/badge/Tableau-Storytelling-e97627?style=for-the-badge&logo=tableau)](https://github.com/)

---

## 📌 Project Overview  
This project delivers a comprehensive analysis of global restaurant data from **Zomato**, generating actionable business insights. It showcases a complete **data analytics lifecycle**, starting from data cleaning and transformation in **SQL** and **Excel**, to advanced modeling and visualization using **Power BI** and **Tableau**.

---

## 🎯 Project Objectives  

- 🧹 **Data Cleaning:** Standardized formats, handled missing values, and removed inconsistencies  
- 🧱 **Data Modeling:** Built a structured data model with a custom **Calendar Table**  
- 💱 **Currency Conversion:** Unified global pricing into **USD** for cross-country comparison  
- 📊 **KPI Development:** Designed business metrics to track growth, pricing, and digital adoption  

---

## 🏗️ Schema Design & Data Structure  

```sql
DROP TABLE IF EXISTS zomato_data;

CREATE TABLE zomato_data (
    restaurant_id       INT PRIMARY KEY,
    restaurant_name     VARCHAR(255),
    city                VARCHAR(100),
    country_code        INT,
    average_cost        DECIMAL(10,2),
    currency            VARCHAR(50),
    has_table_booking   VARCHAR(5),
    has_online_delivery VARCHAR(5),
    aggregate_rating    DECIMAL(3,2),
    votes               INT,
    datekey_opening     DATE
);
```

---

## 🧠 Business Problems & Solutions  

### 1️⃣ Dynamic Financial Calendar Creation  
**Objective:** Track expansion patterns using a custom financial year (starting in April)

```sql
SELECT 
    datekey_opening,
    EXTRACT(YEAR FROM datekey_opening) AS Year,
    TO_CHAR(datekey_opening, 'Month') AS Month_Name,
    'Q' || TO_CHAR(datekey_opening, 'Q') AS Quarter,
    CASE 
        WHEN EXTRACT(MONTH FROM datekey_opening) >= 4 
        THEN EXTRACT(MONTH FROM datekey_opening) - 3 
        ELSE EXTRACT(MONTH FROM datekey_opening) + 9 
    END AS Financial_Month
FROM zomato_data;
```

---

### 2️⃣ Global Price Segmentation (USD)  
**Objective:** Categorize restaurants into Budget, Mid-range, and Premium

```sql
SELECT 
    restaurant_name,
    CASE 
        WHEN (average_cost * exchange_rate) <= 20 THEN 'Budget'
        WHEN (average_cost * exchange_rate) BETWEEN 21 AND 60 THEN 'Mid-range'
        ELSE 'Premium'
    END AS price_bucket
FROM restaurants r
JOIN currency_rates c 
ON r.currency = c.currency_code;
```

---

### 3️⃣ Digital Service Adoption Rate (DAX)  
**Objective:** Measure percentage of restaurants offering online delivery

```DAX
Online_Delivery_% = 
DIVIDE(
    CALCULATE(
        COUNT(Zomato[RestaurantID]), 
        Zomato[Has_Online_Delivery] = "Yes"
    ),
    COUNT(Zomato[RestaurantID]),
    0
) * 100
```

---

### 4️⃣ Ratings vs Service Correlation  
**Objective:** Analyze impact of table booking on customer ratings

```sql
SELECT 
    has_table_booking,
    ROUND(AVG(aggregate_rating), 2) AS avg_rating
FROM zomato_data
GROUP BY has_table_booking;
```

---

## 📈 Key Findings  

- 📊 **Growth Trends:** Significant spike in restaurant openings during **Q2** across major Indian markets  
- ⭐ **Customer Preference:** Restaurants with table booking have **15–20% higher ratings**  
- 🌍 **Market Opportunity:** High-performing regions with low online delivery adoption present expansion opportunities  

---

## 🛠️ Tools & Technologies  

- 🗄️ SQL (PostgreSQL)  
- 📊 Power BI (DAX, Data Modeling)  
- 📈 Tableau (Data Visualization & Storytelling)  
- 📑 Excel (Data Cleaning & Transformation)  

---

## 📚 Dataset Description  

The dataset includes global restaurant-level data with key attributes such as:  
- Restaurant details (Name, City, Country)  
- Pricing & currency information  
- Customer ratings and votes  
- Service availability (Online Delivery, Table Booking)  
- Opening dates for trend analysis  

---

## 🧾 Conclusion  

This project highlights how data-driven insights can support strategic decisions in the food-tech industry. By integrating multi-tool workflows and applying business-focused analysis, it uncovers growth patterns, customer behavior, and market opportunities for better decision-making.

---

## 🚀 Future Improvements  

- 🔍 Incorporate real-time data pipelines  
- 🤖 Apply machine learning for demand prediction  
- 🌐 Expand analysis with more granular geographic segmentation  

---

## 💡 Key Learnings  

- End-to-end data analytics workflow execution  
- Business problem solving using SQL & BI tools  
- Data storytelling through dashboards and KPIs  
- Importance of data standardization in global analysis  
