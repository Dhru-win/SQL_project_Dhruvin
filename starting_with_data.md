-- Question 1: How many unique visitors did the website have

-- SQL Queries:

SELECT DISTINCT 
	fullvisitorid
FROM 
	all_sessions

Answer: 

--Query indicates that there are 14223 unique visitors accross all the countries


--Question 2: which products made highest revenue

-- SQL Queries:

SELECT 
	v2productname, sum(units_sold*unit_price) AS product_revenue
FROM 
	all_sessions als
	JOIN analytics an ON als.fullvisitorid = an.fullvisitorid
WHERE
	units_sold IS NOT NULL
GROUP BY 
	v2productname
ORDER BY 
	product_revenue DESC
Answer:

-- "Waze Baby on Board Window Decal", "Google Collapsible Pet Bowl" and "7&quot; Dog Frisbee" bought in the most revenue so far across all the demographics

-- Question 3: What is the source of visitors (channel grouping)and what is the average revenue generated through the perticular grouping channel.

--SQL Queries:

SELECT
	count(*), channelgrouping, avg(revenue)
FROM 
	analytics
GROUP BY 
	channelgrouping
ORDER BY 
	count(*) DESC

Answer:

-- Most visitors landed the site through organic search whereas visitors who accessed site through display bought the most revenue on average

Question 4: What are the most selling items

-- SQL Queries

SELECT 
	v2productname, sum(units_sold) as total_units_sold
FROM
	 all_sessions als
	JOIN analytics an ON als.fullvisitorid = an.fullvisitorid
WHERE 
	units_sold IS NOT NULL
GROUP BY 
	v2productname
ORDER BY
	 total_units_sold DESC

Answer:

"7&quot; Dog Frisbee", "Google Collapsible Pet Bowl" and "Straw Beach Mat" were the most selling items of all time accross all regions
