
# SQL:  Structured Query Language  Exercise

### Getting Started
1. Go to BigQuery UI https://console.cloud.google.com/bigquery
2. Add in the public data sets. 
	3. Click the Add Data icon
	4. Add any dataset
	5. `bigquery-public-data` should become visible and populate in the BigQuery UI. 
3. Add your queries where it says [YOUR QUERY HERE].
4. Make sure you add your query in between the triple tick marks. 
---

For this section of the exercise we will be using the `bigquery-public-data.austin_311.311_service_requests`  table. 

5. Write a query that tells us how many rows are in the table. 
	```
	SELECT count  (*)
FROM `bigquery-public-data.austin_311.311_service_requests` LIMIT 1000
	```

7. Write a query that tells us how many _distinct_ values there are in the complaint_description column.
	```
	 SELECT count ( distinct complaint_description) from `bigquery-public-data.austin_311.311_request` 
	```
  
8. Write a query that counts how many times each owning_department appears in the table and orders them from highest to lowest. 
	``` 
	SELECT  owning_department, count( owning_department) FROM `bigquery-public-data.austin_311.311_request` group by owning_department order by count( owning_department ) desc
	```

9. Write a query that lists the top 5 complaint_description that appear most and the amount of times they appear in this table. (hint... limit)
	```
	SELECT complaint_description, COUNT(complaint_description ) AS `value_occurrence` FROM `bigquery-public-data.austin_311.311_request` GROUP BY complaint_description ORDER BY `value_occurrence` DESC LIMIT 5;
	  ```
10. Write a query that lists and counts all the complaint_description, just for the where the owning_department is 'Animal Services Office'.
	```
	SELECT complaint_description, COUNT(complaint_description ) AS `value_occurrence` FROM `bigquery-public-data.austin_311.311_request` WHERE owning_department = 'Animal Services Office' GROUP BY complaint_description
	```

11. Write a query to check if there are any duplicate values in the unique_key column (hint.. There are two was to do this, one is to use a temporary table for the groupby, then filter for values that have more than one count, or, using just one table but including the  `having` function). 
	```
	select unique_key , count( unique_key) as occ from `bigquery-public-data.austin_311.311_request` group by unique_key having occ >= 1
	```


### For the next question, use the `census_bureau_usa` tables.

1. Write a query that returns each zipcode and their population for 2000 and 2010. 
	```
	SELECT distinct zipcode, sum(population) as pop FROM `bigquery-public-data.census_bureau_usa.population_by_zip_2000` group by zipcode order by zipcode;
	SELECT distinct zipcode, sum(population) as pop FROM `bigquery-public-data.census_bureau_usa.population_by_zip_2010` group by zipcode order by zipcode;
```

### For the next section, use the  `bigquery-public-data.google_political_ads.advertiser_weekly_spend` table.
1. Using the `advertiser_weekly_spend` table, write a query that finds the advertiser_name that spent the most in usd. 
	```
	SELECT advertiser_name, sum(spend_usd) FROM `bigquery-public-data.google_political_ads.advertiser_weekly_spend` group by advertiser_name order by sum(spend_usd) desc limit 1
	```
2. Who was the 6th highest spender? (No need to insert query here, just type in the answer.)
	```
	SELECT
  advertiser_name,
  SUM(spend_usd)
FROM
  `bigquery-public-data.google_political_ads.advertiser_weekly_spend`
GROUP BY
  advertiser_name
ORDER BY
  SUM(spend_usd) DESC
LIMIT
  1 OFFSET 5
	```

3. What week_start_date had the highest spend? (No need to insert query here, just type in the answer.)
	```
1	
2020-02-23
7082900
	```

4. Using the `advertiser_weekly_spend` table, write a query that returns the sum of spend by week (using week_start_date) in usd for the month of August only. 
	```
	SELECT
  week_start_date ,
  sum(spend_usd)
FROM
  `bigquery-public-data.google_political_ads.advertiser_weekly_spend`
WHERE extract(month from week_start_date) = 8
GROUP BY
  week_start_date 
ORDER BY
  sum(spend_usd) DESC
	```
6.  How many ads did the 'TOM STEYER 2020' campaign run? (No need to insert query here, just type in the answer.)
	```
	49
	```
7. Write a query that has, in the US region only, the total spend in usd for each advertiser_name and how many ads they ran. (Hint, you're going to have to join tables for this one). 
	```
		[YOUR QUERY HERE]
	```
8. For each advertiser_name, find the average spend per ad. 
	```
	SELECT
  advertiser_name ,
  avg(spend_usd)
FROM
  `bigquery-public-data.google_political_ads.advertiser_weekly_spend`
  group by advertiser_name 
	```
10. Which advertiser_name had the lowest average spend per ad that was at least above 0. 
	``` 
	SELECT
  advertiser_name,
  AVG(spend_usd) AS av
FROM
  `bigquery-public-data.google_political_ads.advertiser_weekly_spend`
  where spend_usd  > 0
GROUP BY
  advertiser_name
ORDER BY
  av ASC
 ```
## For this next section, use the `new_york_citibike` datasets.

1. Who went on more bike trips, Males or Females?
	```
	males
	```
2. What was the average, shortest, and longest bike trip taken in minutes?
	```
Row	average_dur	min_dur	max_dur	
1	16			1.0		325167
	```

3. Write a query that, for every station_name, has the amount of trips that started there and the amount of trips that ended there. (Hint, use two temporary tables, one that counts the amount of starts, the other that counts the number of ends, and then join the two.) 
	```
	WITH
  T AS (
  SELECT
    DISTINCT start_station_id,
    COUNT(start_station_id ) AS start_station_id_count
  FROM
    `bigquery-public-data.new_york_citibike.citibike_trips`
  GROUP BY
    start_station_id ),
	TT AS (
  SELECT
    DISTINCT end_station_id ,
    COUNT(end_station_id  ) AS end_station_id_count
  FROM
    `bigquery-public-data.new_york_citibike.citibike_trips`
  GROUP BY
    end_station_id  ) 
   select * from T  join TT on T.start_station_id  = TT.end_station_id
	```
# The next section is the Google Colab section.  
1. Open up this [this Colab notebook](https://colab.research.google.com/drive/1kHdTtuHTPEaMH32GotVum41YVdeyzQ74?usp=sharing).
2. Save a copy of it in your drive. 
3. Rename your saved version with your initials. 
4. Click the 'Share' button on the top right.  
5. Change the permissions so anyone with link can view. 
6. Copy the link and paste it right below this line. 
	* YOUR LINK: https://colab.research.google.com/drive/1x2Z9BNpxycALkvq4BZ_2cNwfIOlsCnla?usp=sharing ________________________________
9. Complete the two questions in the colab notebook file. 
