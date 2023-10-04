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

3. Replaced null values in **productrevenue** column by imputing values from **product price** and **productquantity**.

4. Droped **item revenue** and **itemquantity** columns as all the values are null

5. Changed data types of certain columns that were set to incorrect values. 
while importing data from the csv files. 

``` SQL
ALTER TABLE all_sessions 
	ALTER COLUMN productprice TYPE float
	USING productprice::float;
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


1. updating not set values in country to null

``` SQL
update 
	all_sessions
set
	country = NULL
Where
	country = '(not set)'
```

2. Replaced null values in the revenue column in all the visits where sales happened by calculating product of unit_price and units_sold

``` SQL 
UPDATE analytics
SET revenue = (units_sold * unit_price)  
WHERE revenue is NULL AND
	unit_price IS NOT NULL AND 
	units_sold IS NOT NULL 
```

3. In all the columns that contain currency values divided each value by 10,00,000 to remove trailing four zeros and add decimal at 100th place.

``` SQL
UPDATE analytics
	SET revenue = revenue/1000000;

update analytics
	set unit_price = unit_price/1000000;
```

4. Changing data types of **visitstarttime** ,**date**, **timeonsite** to datetime data type

``` SQL
/* For the visitstarttime column */
Alter table analytics ADD COLUMN visit_start_time timestamp;

UPDATE analytics
	SET visit_start_time = to_timestamp(visitstarttime);

/* For date column */

Alter table analytics ADD COLUMN date_of_visit timestamp;

UPDATE analytics
	SET date_of_visit = to_timestamp(date);
```

5. Altering data type of the column from integer to real as it is going to be storing the price values in decimal.

```SQL 
ALTER TABLE analytics
	ALTER COLUMN unit_price TYPE float 
	USING unit_price::float
```