# Cleaning Data

## Cleaning steps taken in each table.

### all_sessions table

1. Changed the "not set" values in **country** and **city** to null.

``` SQL
-- Updating invalid country values
update 
	all_sessions
set
	 country = NULL
Where
	 country = '(not set)'

-- Updating invalid city values

UPDATE all_sessions
SET city = 
 CASE
 WHEN city = 'not available in demo dataset'
     OR city = '(not set)' 
     THEN NULL
ELSE city
 END
```

2. Changed **date**'s data type.

``` SQL
UPDATE all_sessions
	SET date = to_date(date::text, 'yyyymmdd')
```

3. Replaced null values in **product revenue** column by imputing values from **product price** and **product quantity**.

4. Droped **item revenue** and **item quantity** columns as all the values are null

5. Changed data types of certain columns that were set to incorrect values. 
while importing data from the csv files. 

``` SQL
ALTER TABLE all_sessions 
	ALTER COLUMN productprice 
	TYPE real re
	USING productprice::real;
```

6. In all the columns that contain currency values divided each value by 10,00,000 to remove trailing four zeros and add decimal at 100th place.

``` SQL 
UPDATE all_sessions
	SET productprice = productprice/1000000;

UPDATE all_sessions
	SET productrevenue = productrevenue/1000000;

UPDATE all_sessions
	SET totaltransactionrevenue = totaltransactionrevenue/1000000;
```

## analytics table 

1. Duplicate columns in the table needs to be removed which also exist in the all_sessions tabl and are not id.

2. unitprice needs a fix and why do we have a similar column in the

3. In all the columns that contain currency values divided each value by 10,00,000 to remove trailing four zeros and add decimal at 100th place.

``` SQL
UPDATE analytics
	SET revenue = revenue/1000000;

update analytics
	set unit_price = unit_price/1000000;
```

4. changing data types of **visitstarttime** ,**date**, **timeonsite** to datetime data type

``` SQL

```

5. Altering data type of the column from integer to real as it is going to be storing the price values in decimal.

-- updating not set values in country to null

-- analyzing products with price 0