What issues will you address by cleaning the data?
- Getting rid on Null Data
- Figuring out what data is important to me


Queries:
Below, provide the SQL queries you used to clean your data.


To update the unit cost in the anayltics table and divide it by 1,000,000 i ran the following DML code:
```SQL
UPDATE analytics
SET unit_price = (unit_price / 1000000)
```

I ran the same code to update the limited values for revenue in the file. Only 15,355 line items of the ~4,300,000 had revenue data logged. There were 120,000 distinct number of visitor ids, but only 5799 had revenue data corresponding with them.  That data was found with the following code:
```SQL
select count(distinct fullvisitorid) from analytics

select count(distinct fullvisitorid) from analytics
where revenue is not null
```

The divide by 1,000,000 unit conversion was made to the revenue and price columns in the all_sessions table. 

```SQL
UPDATE analytics
SET revenue = (revenue / 1000000)

UPDATE all_sessions
SET totaltransactionrevenue = (totaltransactionrevenue / 1000000),
    productprice = (productprice / 1000000),
    productrevenue = (productrevenue / 1000000),
    transactionrevenue = (transactionrevenue / 1000000)
```
The all_sessions and analytics tables have large swaths of missing data. I used an inner join to combine the two tables on the common fullvisitorid the tables using the following SQL code.

```SQL
SELECT *
FROM analytics anal
JOIN all_sessions als ON anal.fullvisitorid = als.fullvisitorid
WHERE revenue IS NOT null
ORDER BY revenue DESC
```
On the same line item revenue and time on site data between the 2 tables will be different. There is significantly more revenue data in the analytics file so I will go with the information in that data source.  There are only 81 line items of logged revenue data in the all_sessions table vs the ~15,000 line items in the analytics table.

When running the inner join of the all_sessions and analytics tables there are only 833 line items with logged revenue.  However there are 3987 lines of data without logged revenue data, but a units_sold and a unit_price parameter. I then created a second analytics table and ran the following DML code to calculate revenue as the product of unit_price and units_sold. 

```SQL
CREATE TABLE analytics2
as select * from analytics

UPDATE analytics2 
SET REVENUE = (UNITS_SOLD * UNIT_PRICE)
```
In analytics 2 there are 95,000 line items with relevant revenue data vs just 15,000 in the plain analytics file.

The products and sales_report tables look good

sales_by_sku has no missing data, however this table is redundant as the exact same information is included in the sales_report table with the exception of 8 line items.  Sales_report has 454 rows, where sales_by_sku has 462.  The missing line items are found via the following code:

```SQL
SELECT *
FROM sales_by_sku
WHERE productsku NOT IN
		(SELECT productsku
			FROM sales_report)
```

When it came time to create primary keys for the data, there was no issue with the sales or products tables, but the all_sessions and the analytics tables had duplicate data throughout every column.  