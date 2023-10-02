# Cleaning Data

## Cleaning steps to be taken.

### all_sessions table
1. Change the "not set" values in **country** and **city** to null.
2. Change **date**'s data type.
3. Replace null values in **product revenue** column by imputing values from **product price** and **product quantity**.
4. Drop **item revenue** and **item quantity** columns as all the values are null
3. change data types of certain columns that were set to incorrect values. 
while importing data from the csv file. 
   
-- Trailing 99 suggests that it is the pricing stratigy after the decimal. fix the pricing for the products by removing 4 zeros from the back and adding a decimal appropriately
-- Following issues needs to be addressed in analytics table
-- Duplicate columns in the table needs to be removed which also exist in the all_sessions tabl and are not id.
-- unitprice needs a fix and why do we have a similar column in the all_sessions tanle.

Queries:
Below, provide the SQL queries you used to clean your data.

-- changing format of date and time columns to appropriate data types

UPDATE 
	all_sessions
SET date = to_date(date::text, 'yyyymmdd')

-- Product price is in USD and it can not be as high as it is judging from the v2productname.
-- Altering data type of the column from integer to real as it is goning to be storing the price values in decimal

ALTER TABLE all_sessions 
ALTER COLUMN productprice 
TYPE real re
USING productprice::real;

-- the trailing 99 before the 4 zeros suggest that "charm pricing" stratigy is used
-- Hence, removing zeros and placing decimal appropriately

update all_sessions
SET productprice = productprice/1000000;

UPDATE all_sessions
	SET productrevenue = productrevenue/1000000;

update all_sessions
SET totaltransactionrevenue = totaltransactionrevenue/1000000;

UPDATE analytics
	SET revenue = revenue/1000000;
	
update analytics
	set unit_price = unit_price/1000000;
	
-- updating not set values in country to null

update 
	all_sessions
set
	 country = NULL
Where
	 country = '(not set)'

-- updating invalid city values

UPDATE all_sessions
SET city = 
 CASE
 WHEN city = 'not available in demo dataset'
     OR city = '(not set)' 
     THEN NULL
ELSE city
 END

-- analyzing products with price 0

SELECT * 
FROM all_sessions
WHERE productprice <= 0;
