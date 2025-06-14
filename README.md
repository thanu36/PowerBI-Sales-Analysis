# PowerBI-Sales-Analysis
Sales Analysis is an end-to-end data analytics project focused on transforming raw sales data into actionable business insights. The project follows a complete data analysis lifecycle:
<p align="center">
   
🔵 *Understanding the Problem Statement* – Defining business challenges and analytical goals.

🔵 *Project Planning using the AIIMS Grid* – Structuring objectives, metrics, and deliverables.

🔵 *Data Collection* – Extracting source data from a MySQL database.

🔵 *Exploratory Data Analysis (EDA)* – Using SQL to understand the dataset, identify data types, check for nulls, duplicates, and data consistency.

🔵 *Data Cleaning and Transformation (ETL)* – Preparing the data by handling missing values, standardizing formats, and building a structured data model.

🔵 *Dashboard Development* – Creating an interactive and visually compelling Power BI dashboard for real-time sales insights.

This project demonstrates practical skills in SQL, ETL processes, data modeling, and Power BI visualization, and is designed to support data-driven decision-making in a business context.

📑 Table of Contents

1.[Problem statement](#Problemstatement)

2.[Key Features](#KeyFeatures)

3.[Steps Involved](#StepsInvolved)

4.[Dashboard Screenshots](#DashboardScreenshots)

5.[Tools and Technologies Used](#ToolsandTechnologiesUsed)

6.[Key Insights](#KeyInsights)

7.[Conclusion](#Conclusion)

</p>

<br>


<ins>**1.Problem statement**</ins>


<p align="center">

Atliq Hardware Systems is a company that supplies computer hardware and peripherals to wide range of clients across India. They supply products to clients in various regins of India. The company is headquatered in Delhi, with several regional offices spread scross the nation. The sales manager faces several challenges in tracking and analysing sales in rapidly growing and dynamic market.

</p>

**Challanges**
1. Lack of real time insights
Sales manager in each region provides individual reports, making it difficult for director to access overall performance.
   
2. Insufficient Reporting Process
The current process involves receiving numerous Excel files each quarter, which makes tracking sales performance time-consuming and inefficient.

<br>

<ins> 2.**Steps Involved** </ins>

<br>

*a. Project planning using AIIMS grid*

-> Purpose - interactive dashboard that enables real time sales insights and support data driven decison making

-> Stakeholder - Market team, Sales director, IT team, Data Analytics team.

-> End result - interactive dashboard providing latest sales insight

-> Success criteria - Dashboard is user friendly, reflects sales insights and helps stakeholders identify trends

<br>

*b. Data discovery and data analysis*


Once AIMS grid is defined, next step is data discovery. In this step, data analyst team approaches IT team within an organization who owns software system that keep track of sales records. These records are stored in MySQL database. Power BI can be plugged to this database to pull necessary information required for data analysis.

<p align="center">

![Screenshot 2025-05-11 232620](https://github.com/user-attachments/assets/616d6374-4169-449e-ba6c-e7cf8359f36c)

</p>


-> IT team provides the Analytics team  with access to MySQL database

-> The analytics team:

   * Inspects tables (Customers, Products, Transactions, Date, Markets)
   * Verifies data integrity by checking for for proper formatting of data, missing or null values.
<br>

*c Data Loading*

-> Established a connection to the MySQL database using Power BI's built-in connector and authenticated using server credentials.

-> Loaded  data into Power BI's data model for building visualizations and performing further analysis.

<br>


*c. Data Cleaning*

-> ***Removed non Indian Markets*** :  Filtered out rows with New York and Paris from the Markets table since the business is focused in India.
```m
= Table.SelectRows(sales_markets, each ([markets_name] <> "New York" and [markets_name] <> "Paris"))**
```
<br>

-> ***Filtered empty rows*** : Removed rows with missing values to maintain data quality.
```m
= Table.SelectRows(sales_markets, each ([zone] <> ""))
```
<br>

-> ***Filtered invalid transactions*** : Excluded records where Transaction Amount <= 0.

```m
= Table.SelectRows(sales_transactions, each ([sales_amount] <> -1 and [sales_amount] <> 0))
```

<br>

-> ***Currency conversion*** : Added a conditional column to convert USD to INR using a defined exchange rate formula.

```m
= Table.SelectRows(#"Removed -1/0", each ([currency] = "INR#(cr)" or [currency] = "USD#(cr)"))

```

<br>

-> ***Add new column normalised_sales_amount***: To normalize all sales amounts to INR for consistent analysis, especially when the dataset includes mixed currencies.

```m
= Table.AddColumn(#"Cleanup currency", "normalised_sales_amount", each if [currency] = "USD" or [currency] = "USD#(cr)" then [sales_amount]*75 else [sales_amount])
```

<br>


*d. Data Modelling*

-> Established relationship between tables.

-> Model established star schema

![PB1](https://github.com/user-attachments/assets/1ce4c06f-d2da-4ba9-a152-ee449dd6d27c)



<ins> 2.**Performing primary Analysis on Data** </ins>

*1. To display total records are there in transaction table*
```SQL
SELECT count(*) FROM sales.transactions;
```

<br>

*2. show transactions only from Chennai*
```SQL
SELECT * FROM sales.transactions where market_code = "Mark001";
```
<br>

*3. To know how many currency has USD cureency*
```SQL
SELECT * FROM sales.transactions where currency = "USD";
```
<br>

*4.To show transactions in 2020*
```SQL
SELECT sales.transactions.*, sales.date.* 
from sales.transactions 
inner join sales.date
on sales.transactions.order_date = sales.date.date
where year = 2020;
```
<br>

*5.To calculate the sum of revenue in the year 2020*
```SQL
SELECT SUM(sales.transactions.sales_amount)as sum_revenue 
from sales.transactions 
inner join sales.date
on sales.transactions.order_date = sales.date.date
where year = 2020;
```

*6.To calculate sum of revenue in chennai*
```sql

SELECT SUM(sales.transactions.sales_amount)as sum_revenue  
from sales.transactions 
inner join sales.date
on sales.transactions.order_date = sales.date.date
where year = 2020 and market_code = "Mark001";
```

*6.To calculate sum of revenue in 2020*
```sql

SELECT SUM(sales.transactions.sales_amount)as sum_revenue  
from sales.transactions 
inner join sales.date
on sales.transactions.order_date = sales.date.date
where year = 2020;
```

<br>

<ins> 3.**Building Reports** </ins>

*a. Displaying revenue by customer*

![revenue](https://github.com/user-attachments/assets/721b0d07-5d88-414b-ada2-b39e94c87ebb)


*b. Sales quantity by markrt*

![market](https://github.com/user-attachments/assets/76184dbe-b67f-49b0-9e80-52851bea8a39)


*c. Revenue Trend*

![Revenue trend](https://github.com/user-attachments/assets/47db0648-e8bd-491d-99d0-4a4d088603d9)


*d.Top 5 Customers*

![Top 5 Customers](https://github.com/user-attachments/assets/6ac9058a-bcfd-4b51-8237-99c4a3cd5c36)


*e.Top 5 products*

![Top 5 Products](https://github.com/user-attachments/assets/fad66f3a-5d00-431d-87e9-e812a637fa17)



