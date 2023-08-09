# Travel_Agency_Analysis
A full analysis of a Travel Agency datasets to maximize efficiency and productivity.

## INTRODUCTION
A Travel Agency analysis with a Tableau Dashboard

## 1. Clean, Process and Prepare
Converted the assessment_data.db file to a csv file in the SQLite app and imported it into Google Sheets. Reviewed the data in the database and arranged the structural design. I uploaded the dim_advertising_cost.csv file in Big Query SQL workspace. The schema showed the “date” data type is in string and not in “date” format. The “advertising_cost” data type is in string and not in “integer” data type. 
![image](https://github.com/MaimelaT/Travel_Agency_Analysis/assets/139053059/45fe7fd9-beee-4764-87a2-b121fb21df82)
Figure 1: Big Query showing data types in dim_advertising_cost file
The “date” column in a string type indicates a group of characters within a cell is not consistent and may contain letters, numbers or both.
o	Converting the “date” string type to a ‘date’ data type.
SELECT
CAST(date AS DATE)
FROM `rhino-assessment.rhino.dim_advertising_cost`

The “advertising_cost” column in a string type indicates a group of characters within a cell is not consistent and may contain letters, numbers or both.
SELECT
CAST(advertising_cost AS INT)
FROM `rhino-assessment.rhino.dim_advertising_cost`

o	To check if all columns have empty cells:
--Checking for empty cells in the file
SELECT *
FROM `rhino-assessment.rhino.dim_advertising_cost`
WHERE date ='' OR
id IS NULL OR
channel_id IS NULL OR
advertising_cost IS NULL OR
created_at IS NULL OR
updated_at IS NULL

--Checking for duplicates
SELECT id,
COUNT(*) AS count_of_id
FROM `rhino-assessment.rhino.dim_advertising_cost`
GROUP BY id
HAVING COUNT(*) > 1

### Summary of methodology for addressing quality issues and implementation
* I used Big Query SQL to do data cleaning and data profiling to identify data types, inaccuracy, consistency, timelines, null values, typing errors and duplicates in the dim_advertising_cost file.
* Created a metadata and data constraints for the data and documented all changes I made to ensure the same error doesn’t occur again.
* Implementing this methodology will help ensure that financial history datasets are clean, accurate, and reliable, thereby enabling informed decision-making and fostering trust in the data-driven processes.
* Regularly updating and refining this methodology will continually improve data quality and enhance the overall effectiveness of financial analysis and forecasting.

## 2. Creating Tables in SQL


### I used GOOGLE Big query to create a master_roi database.

CREATE VIEW master_roi AS
SELECT
(a.channel_id) AS Markerting_channel,
(a.revenue_incl) AS Monthly_revenue_incl,
(a.revenue_excl) AS Monthly_revenue_excl,
a.cost_to_supplier,
c.advertising_cost
FROM `rhino.fact_transaction`a
INNER JOIN
`rhino.dim_advertising_cost`c ON a.channel_id = c.channel_id

### I used SQLite Online to generate a view. To showcase my skills with different SQL programs.

CREATE VIEW master_transaction AS
SELECT
a.transaction_id,
b.brand,
c.region,
c.country,
d.channel,
e.campaign_name,
f.consultant_name,
g.customer_name,
h.travel_destination,
a.revenue_excl,
a.cost_to_supplier,
a.date
FROM   fact_transaction a
INNER JOIN dim_location c ON c.id=a.location_id
INNER JOIN dim_consultant f ON f.id=a.travel_destination_id
INNER JOIN dim_brand b ON b.id=a.brand_id
INNER JOIN dim_channel d ON d.id=a.channel_id
INNER JOIN dim_campaign e ON e.id=a.campaign_id
INNER JOIN dim_customer g ON g.id=a.customer_id
INNER JOIN dim_travel_destination h ON a.travel_destination_id=h.id
;


## 3. Tableau

