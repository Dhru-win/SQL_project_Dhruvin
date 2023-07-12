-- What are your risk areas? Identify and describe them.

-- the risk are in the data base are primarily with the calculation with the revenue of products, additionally, there are concerns in the values of date and time fields of the data where there are a lot of values out of range.
--Moreover, there are a lot of null values in the country and city  column which can contribute to invalid results. it is especially important to have those value correct as there are a lot of demographics related questions that need to be addresed.
--Duplicate columns and totally empty columns are another alarming issues in the data which needs to be looked into.

QA Process:

-- Describe your QA process and include the SQL queries used to execute it.

-- To ensure that the revenue generated is same product of the units sold and product price
-- By running the above query it appears there is a difference between in the product of quantity ordered and its price that is the actual revenue and the revenue which is displayed in the revenue column. Which make this. QA issue which needs to be addressed.

SELECT 
	unit_price, units_sold, 
	units_sold * unit_price AS actual_revenue, revenue
FROM 
	analytics
Where
	 units_sold > 0 AND revenue > 0

--  time coulmn in the all_sessions table contains unreliable values
-- following query should not return any results whereas there are over 1300 records of data that make the time coulmn highly unreliable for data analytics

SELECT 
	time 
FROM 
	 all_sessions
where
	 time > 235959


--Following query is run to ensure that there are no sentiment score values over 100 and under 0
-- the results from the query that there are 41 rows in the data set that contain invalid values

SELECT *
FROM
	 products
WHERE 
	sentimentscore > 100 OR sentimentscore < 0


-- Sentiment magnitude sjould not be over 1 or under -1 following query will check that there is no data outside these 
-- As it turns out there are316 rows containing invalid value

SELECT
	sentimentmagnitude 
FROM
	products
WHERE
	sentimentmagnitude > 1 OR sentimentmagnitude < -1