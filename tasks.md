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



    
