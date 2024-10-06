# REAL_ESTATE_DB
This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze real estate sales data. The project involves setting up a real estate management database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries and visualization using power bi.
**REAL ESTATE MANAGEMENT ANALYSIS USING SQL AND POWER BI**
1. Database Setup:
Our real estate SQL project involves querying multiple tables: Sales_Table, Property_Table, Client_Table, and Expense_Table.
•	In our real estate database, we queried tables which are imported csv flat files that are sales, property, client, and expense datasets.
**SQL Queries**:
sql
Copy code
SELECT * FROM [dbo].[Sales_Table];
SELECT * FROM [dbo].[Property_Table];
SELECT * FROM [dbo].[Client_Table];
SELECT * FROM [dbo].[Expense_Table];
This is the foundation of our analysis, ensuring we have access to relevant data.
2. **Data Exploration & Cleaning**:
In our real estate project, we explore our tables and ensure the data is clean.
We start by determining the number of properties sold, revenue from sales, and total expenses to get a sense of the data.
SQL Queries for Exploration:
•	Properties Sold:
sql
Copy code
SELECT COUNT(*) AS PropertiesSold
FROM Property_Table
WHERE Property_Table.Status = 'Sold';
•	Revenue from Sold Properties:
sql
Copy code
SELECT SUM(Property_Table.Price) AS Revenue
FROM Property_Table
WHERE Property_Table.Status = 'Sold';
•	Total Expense:
sql
Copy code
SELECT SUM(Amount) AS TotalExpense 
FROM Expense_Table;

3. **Exploratory Data Analysis (EDA)**:
In our real estate project, we  perform EDA, such as calculating revenue, expense, and income, as well as looking at sales trends by year and month.
Example SQL Queries for EDA:
•	Income Calculation:
sql
Copy code
SELECT RevenueTable.Revenue - ExpenseTable.Expense AS Income
FROM 
    (SELECT SUM(Property_Table.Price) AS Revenue
     FROM Property_Table
     WHERE Property_Table.Status = 'Sold') AS RevenueTable,
    (SELECT SUM(Expense_Table.Amount) AS Expense
     FROM Expense_Table) AS ExpenseTable;
•	Monthly Sales Analysis:
sql
Copy code
SELECT 
    YEAR(s.SaleDate) AS SaleYear,
    CASE 
        WHEN MONTH(s.SaleDate) = 1 THEN 'January'
        WHEN MONTH(s.SaleDate) = 2 THEN 'February'
        WHEN MONTH(s.SaleDate) = 3 THEN 'March'
        WHEN MONTH(s.SaleDate) = 4 THEN 'April'
        WHEN MONTH(s.SaleDate) = 5 THEN 'May'
        WHEN MONTH(s.SaleDate) = 6 THEN 'June'
        WHEN MONTH(s.SaleDate) = 7 THEN 'July'
        WHEN MONTH(s.SaleDate) = 8 THEN 'August'
        WHEN MONTH(s.SaleDate) = 9 THEN 'September'
        WHEN MONTH(s.SaleDate) = 10 THEN 'October'
        WHEN MONTH(s.SaleDate) = 11 THEN 'November'
        WHEN MONTH(s.SaleDate) = 12 THEN 'December'
    END AS SaleMonth,
    SUM(p.Price) AS TotalRevenue
FROM Sales_Table s
JOIN Property_Table p ON s.PropertyID = p.PropertyID
WHERE p.Status = 'Sold'
GROUP BY YEAR(s.SaleDate), MONTH(s.SaleDate)
ORDER BY SaleYear, MONTH(s.SaleDate);
This EDA allows us to spot seasonal patterns and revenue trends across months and years.

4. **Business Analysis**:
In our real estate project, we analyz revenue by country, client purchases, property listings by price, and more.
SQL Queries for Business Insights:
•	Revenue by Country:
sql
Copy code
SELECT Country, SUM(Price) AS Revenue 
FROM Property_Table 
WHERE Status = 'Sold'
GROUP BY Country;
•	Client Purchase Insights:
sql
Copy code
SELECT c.Name, c.Occupation, COUNT(s.SaleID) AS Purchases
FROM Sales_Table s
JOIN Client_Table c ON s.ClientID = c.ClientID
WHERE s.Payment_Status = 'Paid'
GROUP BY c.Name, c.Occupation
ORDER BY Purchases DESC;
•	Most Expensive Properties by Location and Year:
sql
Copy code
SELECT Year(ListedDate) AS Year, MAX(Price) AS MaxPrice, Country, Location 
FROM Property_Table 
GROUP BY Year(ListedDate), Country, Location 
ORDER BY Year(ListedDate);
•	Yearly Sales Analysis:
sql
Copy code
SELECT 
    YEAR(s.SaleDate) AS Year, 
    SUM(p.Price) AS TotalRevenue
FROM 
    Property_Table p
JOIN 
    Sales_Table s ON p.PropertyID = s.PropertyID
WHERE 
    p.Status = 'Sold'
GROUP BY 
    YEAR(s.SaleDate)
ORDER BY 
    Year;

6.	**FINDINGS**
Customer Demographics: The dataset includes clients from various occupations and demographics, with sales distributed across different property types and locations.
•	High-Value Transactions: Several property sales had a total price greater than 1,000,000, indicating premium properties sold to high-net-worth clients.
•	Sales Trends: Monthly and yearly analysis shows variations in property sales, helping identify peak seasons for property transactions.
•	Client Insights: The analysis identifies the top clients by purchase volume and revenue, as well as the most popular property locations and types.
7.	**Reports**
•	Sales Summary: A detailed report summarizing total revenue, client demographics, and property performance based on location and type.
•	Trend Analysis: Insights into property sales trends across different months and years, identifying peak selling periods.
•	Client Insights: Reports on top clients by total purchases, client occupations, and unique client counts for different property types and locations.
8.	**Conclusion**
This project serves as a comprehensive anlysis for querying using SQL for data analysts in the real estate domain, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding property sales patterns, client behavior and occupation, and the performance of properties across different regions and price points.

**LOADING INTO POWER BI**
NOTE 1: THE QUERIED REAL ESTATE MANAGEMENT IS SAVED IN OTHER TO VALIDATE WHEN LOADED INTO POWER BI FOR DAX MEASURES AND VISUALIZATION

NOTE 2: WE GET DATA BY IMPORTING 4 EXCEL CSV FILE VIA SQL SERVER (THE EXACT QUERIED DATA IN SQL)

NOTE 3: WE HAD 4 TO 1 MODEL VIEW AND MEASURES WAS PREPARED FOR VISUALIZATION
**Comprehensive Power BI Dashboard for Real Estate Management**
Page 1: Property Overview
•	Map Visualization:
o	Property sold are easily located via zoom in, allowing viewers to easily visualize where properties are situated and compare sales across different regions.
•	New Slicers:
o	Slicers for years spanning 2022-2024 show that:
	2023 had the highest revenue and property sales.
	David Client and Emily Davis led the property sales in 2023, each selling 10 properties.

Page 2: Property Detail
•	KPI Cards:
o	Featured metrics include:
	Revenue, expenses, income, and properties sold.
	These provide quick insights into overall business performance.
•	Brokerage Information Card:
o	Displays the brokerage associated with each property sale.
•	Table:
o	Provides in-depth information about properties sold, including their specific details like sale date, price, payment status, and location.
•	Key Influencers Diagram:
o	Identifies trends in property types, highlighting that:
	Condos had the highest percentage of sales in 2023 and 2024.
	Single apartments led the market in 2022.

Visualizations and Insights
•	Revenue by Sale Method:
o	Displayed through a bar chart:
	2023: Brokers accounted for $19M+ in revenue.
	2022: Brokers led with $11.8M.
	2024: Online sales dominated with $9.5M.
•	Revenue by Property Type:
o	Visualized via a bar chart:
	Condos had the highest revenue percentage in 2023 and 2024.
	Single apartments were the top-selling property type in 2022.
•	Revenue by Month:
o	A bar chart illustrates the revenue trends across different months, helping identify seasonality in sales.
•	Most Expensive Property:
o	This uses an image URL to display the most expensive property for the selected year, with details about its price, location, and sale date, dynamically updated by the slicer.

Page 3: Location & Client Insights
•	Location, Sale Date, Price, and Payment Details:
o	Visualized using a table format for easy access to transactional information for each property.
•	Revenue by Country:
o	Displayed using a shape map with zoom-in features to explore revenue distribution across countries.
o	Additionally, a table offers detailed revenue breakdowns by country.
•	Best Clients Table:
o	Highlights the top clients and their occupations over the years:
	Noah Moore generated the highest revenue in 2022.
	David Client and Emily Davis were top clients in 2023.
	Emily Davis led revenue in 2024.

Development Stages
1.	Data Transformation:
o	Automated ETL process through Power Query for seamless data extraction, transformation, and loading.
o	Applied parameters to enable flexible data sourcing.
2.	Data Modeling:
o	Efficient and optimized data model following best practices for relational database design.
o	Optimized for performance and scalability.
3.	DAX Analysis:
o	Employed DAX functions to calculate critical metrics such as revenue, expenses, and income.
o	Advanced DAX measures enabled deep insights into property sales, client performance, and market trends.
4.	Dynamic Reporting:
o	Developed interactive, user-friendly dashboards with slicers, buttons, and interactive visuals for seamless navigation.
o	Insights are designed to empower decision-makers with a clear view of performance and operational data.
________________________________________
**Conclusion**
This Power BI dashboard provides Real Estate Managment with a comprehensive overview of property sales, revenue trends, and client insights over the years 2022-2024. By combining dynamic visualizations, advanced DAX calculations, and detailed KPIs, this dashboard empowers stakeholders to make data-driven decisions, optimize operations, and identify growth opportunities in the real estate market.
