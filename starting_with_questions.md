Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

``` SQL

SELECT country, 
    city, 
    SUM(revenue) AS "Total Recorded Revenue"
FROM analytics2 anal
JOIN all_sessions als ON anal.fullvisitorid = als.fullvisitorid
WHERE revenue IS NOT null 
    AND city != 'not available in demo dataset'
GROUP BY country, 
    city
ORDER BY SUM(revenue) DESC

```

Answer:

The highest transaction revenue is in the US, with the highest transaction revenue from cities being in Mountain View, San Bruno, New York, Chicago and Sunnyvale.  This is using the analytics 2 data set with the calculated revenue from units_sold and unit price.   

Using the analytics data provided in the ecommerce data set the highest transaction revenue is still the US, and the top 5 cities are the same, but the sequencing is Mountain View, San Bruno, New York, Sunnyvale, Chicago.

With the analytics2 dataset there is revenue data from 24 countries, the top 5 being: the US, Canada, Hong Kong, Japan and the UK.  In the analytics data set there is only revenue data for 4 countries: the US, Canada, Japan and Switzerland (total recorded revenue decreasing through the order.) 

**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

``` SQL
SELECT country, 
	city, 
	ROUND(AVG(units_sold),2) AS "Avg No. of Products Ordered"
FROM analytics anal
JOIN all_sessions als ON anal.fullvisitorid = als.fullvisitorid
WHERE units_sold IS NOT null
GROUP BY country, 
    city
ORDER BY ROUND(AVG(units_sold),2) desc
```

Answer:

On average most cities only order one product per order.  The results show that 68 out of 102 cities in the data set order 1 product at a time on average.  The majority of the cities that are ordering more than one product per order are in the US. 4 of the top 5 are the US cities: San Bruno, Unknown city due to missing data, Mountain View and San Jose with average product orders of 52.67, 29.12, 16.17 and 8.57 respectively. The 4th highest products ordered is from Czechia with 15.18.  The information about which city the purchases were made from was included in the data. 

When looking at just the countries in the data set, 33 out of 42 countries only order 1 product at a time on average.  The highest average products per order is the United States with 19.24, the second is Czechia with 15.18, third is Mexico with 1.83, fourth is Canada with 1.59 and fifth is Bulgaria with 1.50.  When running the max aggregate function we can see that the US average is so high becasue of a 1000 product order outlier.  





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

```SQL
SELECT country, 
	v2productcategory,
	SUM(units_sold) as "Units Sold"
FROM analytics anal
JOIN all_sessions als ON anal.fullvisitorid = als.fullvisitorid
WHERE units_sold IS NOT null 
	and v2productcategory != '(not set)'
GROUP BY country, v2productcategory 
ORDER BY SUM(units_sold) DESC
```

Answer:

Like the rest of the data set, the avaiable data is very US dominated.   The majority of the units sold are pet asscessories, followed by office notebooks, fun accessories and apparel.



**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

```SQL
SELECT country, 
	v2productname,
    v2productcategory,
	MAX(units_sold) AS "Units Sold"
FROM analytics anal
JOIN all_sessions als ON anal.fullvisitorid = als.fullvisitorid
WHERE units_sold IS NOT null
GROUP BY country, 
	v2productname,
    v2productcategory
ORDER BY country, 
	MAX(units_sold) DESC
```


Answer:

The US results are once again dominated by pet products. 



**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

``` SQL

SELECT country, 
	city, 
	SUM(revenue) AS "Total Recorded Revenue"
FROM analytics2 anal
JOIN all_sessions als ON anal.fullvisitorid = als.fullvisitorid
WHERE revenue IS NOT null
	AND country = 'United States'
	AND city != 'not available in demo dataset'
	AND city != '(not set)'
GROUP BY country, 
	city
ORDER BY SUM(revenue) DESC

```


Answer:


For question 5 I used almost the same SQL query as question 1, but I chose to only look at the data from the United States.   Since the data is so skewed towards the states and very sparse for other countries, it is the most meaningful data to analyse. 

The revenue generated from cities in the Califronia area are the most significant, but New York, Chicago and Austin are also big revenue generators. 




