
What are your risk areas? Identify and describe them.

The major risk areas with this data came from the amount of null data and the amount of differences between the analytics and the all_sessions file.  To try mitigate inconsistencies I did an inner join of both tables using the fullvisitorid.  The inner join made it so that only fullvisitorid data that both tables shared would be analyzed. 

Then I looked at revenue data from both tables. The revenue column from the analytics table had significantly more data than the revenue columns from the all_sessions table. For all of my analysis going forward I only looked at the revenue data from the analytics table.

As I write this I realize that I made a mistake in my data analysis.  I thought that by joining the analytics and all_sessions table I was able to get the product and customer data from all_sessions table and connect it to the revenue data from the analytics table.

However I didn't realize the fullvisitorid was a duplicated line item. When I went to create my primary keys in the table so that I could then create foreign keys and my ERD diagram at the very end of the day on sunday I realized there are multiple duplicate fullvisitorids.  I looked closer and then grouped things by full vistor id with the following code:

```SQL
SELECT *
FROM analytics anal
JOIN all_sessions als ON anal.fullvisitorid = als.fullvisitorid
WHERE revenue IS NOT null
ORDER BY anal.fullvisitorid
```

 The analytics table would have multiple fullvisitorid rows, which had different unit_price values assigned to the same fullvisitorid.   When the all_sessions.fullvisitorid was joined to the analytics.fullvisitorid, the dataa from the all_sessions table would assign one product to all of those different unit prices. This error means that all of my analysis when grouped by product type is not valid. 


QA Process:
Describe your QA process and include the SQL queries used to execute it.

 In trying to write the file about QA Process I did in fact find an error that makes my data unusable.  while the analysis is not super helpful at this point, atleast my QA skills are on their way.