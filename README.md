# SQL_Power_Bi_Project
SQL_Power_Bi
# SQL_Power_Bi_Project
SQL_Power_Bi
 # 1 Project Overview
This project involves analyzing the total revenue generated by a pizza selling business using Power BI Pro. The data for this analysis is fetched from a SQL database, and the aim is to provide insightful visualizations and reports to help understand the business performance.
 # 2 Data Source
The data used in this project is fetched from a SQL database. It includes the following tables:

Sales: Contains information about each pizza sale, including the date, pizza type, quantity, and revenue.
Pizzas: Contains details about the different types of pizzas available, including their names and prices.
Customers: Contains information about the customers, including their IDs and names.

# 3 PizzaSellerRevenueAnalysis

├── data/                   # SQL scripts or CSV files for sample data
├── reports/                # Power BI reports and dashboards
├── src/                    # Source code for data fetching and processing
├── README.md               # Project README file


# 4 Data Cleaning:

Ensure the data is clean and consistent.
Data Transformation:   Transform the data to create meaningful metrics and dimensions.
Revenue Analysis:  Analyze total revenue, revenue by pizza type, and revenue by customer.


# 5 SQL Query
select * from pizza_sales
--KPI'S Requirment

--Q1. total revenue

select SUM( total_price ) from pizza_sales
select SUM( total_price ) as Total_Revenue from pizza_sales

--Q2. Avg order value

select SUM( total_price )/ COUNT(DISTINCT  order_id) as Avg_Order_Value from pizza_sales

--Q3. Total Pizza sold

select SUM(quantity) as Total_pizza_sold from pizza_sales

--Q4. Total order

select COUNT(DISTINCT order_id) as Total_order from pizza_sales

--Q5. Avg Pizza per order 

select CAST(SUM( quantity ) as DECIMAL(10,2))/
CAST(COUNT(DISTINCT  order_id) as decimal(10,2)) from pizza_sales -- 

select CAST( CAST(SUM( quantity ) as DECIMAL(10,2))/
CAST(COUNT(DISTINCT  order_id) as decimal(10,2))as Decimal (10,2))as avg_pizza_per_order from pizza_sales-- this cast for  entair value  

-- Chart Requirment

--Daily trend for total order

select DATENAME(DW,order_date) as Order_day, count(distinct order_id) as Total_orders
from pizza_sales
group by DATENAME(DW,order_date)

--Hourly trend for total order

select DATENAME(MONTH,order_date) as Month_name  ,COUNT(distinct order_id) as Total_order 
From pizza_sales
group by DATENAME(MONTH,order_date)

-- if you want this order showing accending or deccending order
select DATENAME(MONTH,order_date) as Month_name  ,COUNT(distinct order_id) as Total_order 
From pizza_sales
group by DATENAME(MONTH,order_date)
ORDER BY  Total_order DESC

-- percentage of sales by pizza category

SELECT pizza_category ,SUM(total_price) as total_sales, SUM( total_price ) * 100 /(SELECT SUM(total_price) FROM pizza_sales) as PCT
from pizza_sales 
group by pizza_category

-- percentage of sales by pizza size
-- if you want show only 2 decimal number u  can CAST
SELECT pizza_size ,CAST (SUM(total_price)AS decimal(10,2)) as total_sales, CAST(SUM( total_price ) * 100 
/(SELECT SUM(total_price) FROM pizza_sales)as DECIMAL(10,2))AS PCT
from pizza_sales 
group by pizza_size

--Top 5 Best Seller by revenue Total quantity and toal order 

select TOP 5 pizza_name, SUM(total_price) as Total_revenue from pizza_sales group by pizza_name
order by Total_revenue


-- this is the bottom 5 

select TOP 5 pizza_name, SUM(total_price) as Total_revenue from pizza_sales group by pizza_name
order by Total_revenue desc

-- and same for quantity 
select TOP 5 pizza_name, SUM(quantity) as Total_Quantity from pizza_sales group by pizza_name
order by Total_Quantity desc
