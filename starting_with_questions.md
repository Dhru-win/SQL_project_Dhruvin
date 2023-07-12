-- Answer the following questions and provide the SQL queries used to find the answer.

    
-- **Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

-- total transaction revenues by country

WITH updated_revenue_table AS (

	SELECT 
		city, country, units_sold, unit_price, units_sold*unit_price AS actual_revenue
	FROM 
		analytics
		JOIN 
		all_sessions ON analytics.visitid = all_sessions.visitid
	WHERE units_sold IS NOT NULL AND country IS NOT NULL
	)
	
select
	 sum(actual_revenue) AS totalByCountry, country
from 
	updated_revenue_table
where
	actual_revenue IS NOT NULL AND country IS NOT NULL
group by
	country
ORDER BY 
	totalByCountry DESC


-- Highest amount of revenue generating transactions by country
WITH updated_revenue_table AS (

	SELECT 
		city, country, units_sold, unit_price, units_sold*unit_price AS actual_revenue
	FROM 
		analytics
		JOIN 
		all_sessions ON analytics.visitid = all_sessions.visitid
	WHERE units_sold IS NOT NULL AND country IS NOT NULL
	)
	
select
	 count(actual_revenue) AS countByCountry, country
from 
	updated_revenue_table
where
	actual_revenue IS NOT NULL AND country IS NOT NULL
group by
	country
ORDER BY 
	countByCountry DESC

-- customers in the following bought in the most revenue cities that bought in the most revenue

WITH updated_revenue_table AS (

	SELECT 
		city, country, units_sold, unit_price, units_sold*unit_price AS actual_revenue
	FROM 
		analytics
		JOIN 
		all_sessions ON analytics.visitid = all_sessions.visitid
	WHERE units_sold IS NOT NULL AND city IS NOT NULL
	)
	
select
	 sum(actual_revenue) AS totalByCity, city
from 
	updated_revenue_table
where
	actual_revenue IS NOT NULL AND city IS NOT NULL
group by
	city
ORDER BY 
	totalByCity DESC

-- highest aversge revenue per transaction by city

WITH updated_revenue_table AS (

	SELECT 
		city, country, units_sold, unit_price, units_sold*unit_price AS actual_revenue
	FROM 
		analytics
		JOIN 
		all_sessions ON analytics.visitid = all_sessions.visitid
	WHERE units_sold IS NOT NULL AND city IS NOT NULL
	)
	
select
	 avg(actual_revenue) AS avgByCity, city
from 
	updated_revenue_table
where
	actual_revenue IS NOT NULL AND city IS NOT NULL
group by
	city
ORDER BY 
	avgByCity DESC

-- highest number of transaction per city

WITH updated_revenue_table AS (

	SELECT 
		city, country, units_sold, unit_price, units_sold*unit_price AS actual_revenue
	FROM 
		analytics
		JOIN 
		all_sessions ON analytics.visitid = all_sessions.visitid
	WHERE units_sold IS NOT NULL AND city IS NOT NULL
	)
	
select
	 count(actual_revenue) AS countByCity, city
from 
	updated_revenue_table
where
	actual_revenue IS NOT NULL AND city IS NOT NULL
group by
	city
ORDER BY 
	countByCity DESC

Answer:
Most of the visitors who made a purchace reside in USA followwed by japan and canada. When it comes to the countries from which most revenue was generated than USA is still on the top but Sweden took the second place followed by Mexico. 
If we narrow down our criteria to the cities, then we can see that New York, Sunnyvale and Mountain View brought in the most revenue respectively. Although, the highest number of transaction were in Mountain View followed by Sunnyvale and New York.
Furthermore, on an average visitors in New York, seattle and Pittsburgh spent the most money per transaction.

--**Question 2: What is the average number of products ordered from visitors in each city and country?**

SQL Queries:

-- Average orders per country

SELECT country,
	avg(total_ordered) AS avgOrders
FROM sales_report AS sr
	JOIN all_sessions AS als ON als.productsku = sr.productsku
WHERE 
	country IS NOT NULL
GROUP BY 
	country

--Average orders per city

SELECT city,
	avg(total_ordered) AS avgOrders
FROM sales_report AS sr
	JOIN all_sessions AS als ON als.productsku = sr.productsku
WHERE city != '(not set)' or country IS NOT NULL
GROUP BY city

Answer:

On an average, Visitors from Saudi Arabia ordered the most items followed by Kuwait and Oman based on country. If we look at it on the basis of city then Bruno, Riyadh and Rexburg has highest number of products ordered.

--**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**

SQL Queries:

SELECT
	 v2productcategory, country, count(*)
FROM
	 all_sessions
Group by
	v2productcategory, country
ORDER BY
	 country	
	 
Answer:

--The above query returns the list of categories ordered from each country and shows what categories are most popular

--**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**

SQL Queries:

SELECT
	 als.country, pr.name, pr.orderedquantity,
   	MAX(orderedquantity) over (partition by country order BY orderedquantity DESC)
FROM 
	all_sessions als
	JOIN products pr on als.productsku = pr.sku
Group BY als.country, pr.name, pr.orderedquantity

Answer:

-- The above query arranges the ordered quantity per quantity, listing highest ordered quantity on the top


--**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

select
	 sum(totaltransactionrevenue), country
from 
	all_sessions
where
	 totaltransactionrevenue > 0
group by
	 country


Answer:

-- Results from the following query indicates customers from USA bring in the highest amount of revenue.





