# Gift-2-Northwind-Analysis
## Executive Summary

This project analyses the Northwind retail database to extract actionable business insights across customers, revenue, products, and operations using SQL.

Key findings show that:

The business has a highly global customer base (91 countries).
A small group of repeat customers drives significant activity.
Revenue exceeds $1.26M, indicating strong commercial performance.
There are data quality gaps (missing regions) and operational inefficiencies (pending shipments) that require attention.
Core markets are concentrated in a few countries (USA, Germany, and France).

The analysis demonstrates how SQL can be used to translate raw data into business decisions.

## Project Objectives
Analyse customer distribution and diversity
Evaluate business activity and revenue performance
Identify high-value customers and strong markets
Assess data quality issues
Highlight operational gaps (e.g., unshipped orders)
Support decision-making for marketing, sales, and operations teams

## Dataset
Source: Northwind Database (SQLite)
Tables Used:
Customers
Orders
Order Details
Products
Suppliers
Categories

## Tools & Technologies
SQL (SQLite) – Data extraction & transformation
SQLite Online – Query execution
Excel – Data visualisation & reporting

## Project Workflow
1. Loaded dataset into SQLite
2. Wrote SQL queries based on business questions
3. Extracted and validated results
4. Exported results into Excel
5. Created charts and summaries
6. Interpreted insights for business stakeholders

## Business Questions & Analysis
1. 🌍 Customer Diversity Analysis

Objective: Understand global customer distribution

SELECT DISTINCT country AS customer_country 
FROM Customers;

Insight:

Customers span 91 countries, indicating a highly globalised customer base.
This supports international marketing and expansion strategies.

📊 Suggested Chart: Bar chart – Customers by Country

2. ⚠️ Missing Data Audit

Objective: Identify incomplete customer records

SELECT CustomerID, CompanyName, Country 
FROM Customers 
WHERE Region IS NULL;

Insight:

60 customers (7.23%) have missing region data
This may impact:
Logistics planning
Regional marketing campaigns

📊 Suggested Chart: Pie chart – Missing vs Complete Data

3. 📦 Order Volume Overview

Objective: Measure overall business activity

SELECT COUNT(DISTINCT CustomerID) AS total_customers,
COUNT(OrderID) AS total_orders
FROM Orders;

Insight:

830 total orders placed
Active customer base contributing to consistent transaction volume

📊 Suggested Chart: KPI Cards (Total Customers vs Orders)

4. 💰 Revenue Calculation

Objective: Estimate total revenue

SELECT SUM(UnitPrice * Quantity * (1 - Discount)) 
AS total_revenue 
FROM "Order Details";

Insight:

Total revenue = $1,265,793.04
Indicates strong overall sales performance

📊 Suggested Chart: KPI Card – Total Revenue

5. 📊 Product Performance by Category

Objective: Evaluate product distribution

SELECT CategoryID, COUNT(ProductID) AS product_count 
FROM Products 
GROUP BY CategoryID 
ORDER BY CategoryID ASC;

Insight:

Category 3 (Confections) has the highest variety (13 products)
Category 7 (Produce) has the lowest (5 products)
Suggests uneven product distribution across categories

📊 Suggested Chart: Bar chart – Products per Category

6. ⭐ High-Value Customers

Objective: Identify top-performing customers

SELECT CustomerID, COUNT(*) AS total_orders
FROM Orders  
GROUP BY CustomerID
HAVING total_orders > 10
ORDER BY total_orders DESC;
SELECT COUNT(*) AS high_order_customer_count
FROM (
    SELECT CustomerID
    FROM Orders
    GROUP BY CustomerID
    HAVING COUNT(OrderID) > 10
) AS eligible_customers;

Insight:

28 high-value customers identified
Top contributors:
SAVEA (31 orders)
ERNSH (30 orders)
QUICK (28 orders)
Revenue is concentrated among repeat customers

📊 Suggested Chart: Bar chart – Orders by Top Customers

7. 🚚 Average Order Value (Freight)

Objective: Understand customer cost behavior

SELECT CustomerID, AVG(Freight) AS avg_freight_cost
FROM Orders
GROUP BY CustomerID
ORDER BY avg_freight_cost ASC;

Insight (refined):

Freight costs range from 3.25 to 215.60
Indicates:
Significant variation in shipping behavior
Possible differences in order size, distance, or priority shipping

📊 Suggested Chart: Box plot or distribution chart – Freight Cost

8. 🏭 Supplier Analysis

Objective: Identify key suppliers

SELECT S.SupplierID, COUNT(P.ProductID) AS supply_count
FROM Suppliers S
LEFT JOIN Products P
ON S.SupplierID = P.SupplierID
GROUP BY S.SupplierID
HAVING supply_count > 5;

⚠️ Correction: Your original join was incorrect. Fixed above.

Insight:

No supplier provides more than 5 products
Indicates a highly fragmented supplier network

📊 Suggested Chart: Bar chart – Products per Supplier

9. 🌎 Strong Market Identification

Objective: Identify high-customer regions

SELECT Country AS customer_country, COUNT(CustomerID) AS customer_count
FROM Customers
GROUP BY Country
HAVING customer_count > 5
ORDER BY customer_count DESC;

Insight:
Top markets:

USA (13)
Germany (11)
France (11)
Brazil (9)
UK (7)

These are priority regions for expansion and retention strategies.

📊 Suggested Chart: Bar chart – Customers by Country

10. 🚚 Orders Without Shipment

Objective: Track operational delays

SELECT COUNT(OrderID) AS pending_shipments
FROM Orders
WHERE ShippedDate IS NULL;

Insight:

21 orders are pending shipment
Indicates potential:
Fulfillment delays
Operational inefficiencies

📊 Suggested Chart: KPI Card – Pending Shipments

🔍 Key Insights
🌍 Business operates globally across 91 countries
💰 Revenue exceeds $1.26M
⭐ Repeat customers drive significant value
⚠️ 7.23% data incompleteness (regions missing)
🌎 Core markets are concentrated in 5 countries
🚚 Operational gaps exist with pending shipments
🏭 Supplier base is fragmented, not centralized
📈 Recommendations
Improve data quality processes (mandatory region field)
Focus retention strategies on high-value customers
Expand aggressively in top-performing countries
Optimize logistics to reduce unshipped orders
Evaluate supplier strategy for efficiency and scalability
