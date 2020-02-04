# Quizz 1.

3. In the data set, `bigquery-public-data.new_york.311_service_requests`, how many distinct agency names are there in the agency_name column?

        SELECT count(distinct agency_name) FROM `bigquery-public-data.new_york.311_service_requests`
    
4. Which of the following queries will execute without any errors:
    the comma thing
    
# Quizz 2.

1. Consider the data set `bigquery-public-data.new_york.citibike_trips`, and the column tripduration which gives the length of the trip in seconds. Use distinct tripduration and ORDER BY to find how long the longest ride was in seconds.

        SELECT distinct tripduration
        from `bigquery-public-data.new_york.citibike_trips`

order by tripduration desc 

2. How many distinct birth_year values are there in the `bigquery-public-data.new_york.citibike_trips` data set?

        SELECT count(distinct birth_year)
        from `bigquery-public-data.new_york.citibike_trips` 

3. Use the DISTINCT and ORDER BY on the table `bigquery-public-data.new_york.citibike_trips`, to find the smallest (oldest) birth_year.

        SELECT distinct birth_year
        from `bigquery-public-data.new_york.citibike_trips` 

        order by birth_year asc

4. Use DISTINCT and ORDERBY to find the largest (most recent) birth_year from the data set `bigquery-public-data.new_york.citibike_trips`.

        SELECT distinct birth_year
        from `bigquery-public-data.new_york.citibike_trips` 

        order by birth_year desc
        
5. What are all the possible values from the column usertype in the dataset bigquery-public-data.new_york.citibike_trips?

        SELECT distinct usertype
        from `bigquery-public-data.new_york.citibike_trips` 
        
6. What were the two dates of the most recent and oldest stoptimes in the dataset  `bigquery-public-data.new_york.citibike_trips`?

        SELECT distinct stoptime
        from `bigquery-public-data.new_york.citibike_trips` 

        order by stoptime asc
        
        - then order by desc (it will have another way of showing the range for sure without doing 2 queries)

7. What are the distinct values from the column gender in the table bigquery-public-data.new_york.citibike_trips?

        SELECT distinct gender
        from `bigquery-public-data.new_york.citibike_trips` 
        
8. How many records would this query return? 
SELECT state
FROM   `bigquery-public-data.usa_names.usa_1910_current` 
ORDER BY   state ASC

        check it out

        








    
