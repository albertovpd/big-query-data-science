# Quizz 1. Start.

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
--------------------------------

# Quizz 3. Playing with "where stuff"

1. Consider the data set `bigquery-public-data.austin_bikeshare.bikeshare_trips`. How many unique rides involved the bike with a bikeid of 446.

        SELECT
        COUNT(*) AS num_rides 
        FROM `bigquery-public-data.austin_bikeshare.bikeshare_trips`
        WHERE bikeid = "446"
        
        # be careful, bikeid should be integer but it's string type
        
2. For the bike with bikeid=446, what was the time of its longest ride in minutes?
        
        SELECT
        duration_minutes
        FROM `bigquery-public-data.austin_bikeshare.bikeshare_trips`
        WHERE bikeid = "446"
        ORDER BY duration_minutes DESC

3. How many bike rides started at the station Zilker Park West?

        SELECT
        count(trip_id)
        FROM `bigquery-public-data.austin_bikeshare.bikeshare_trips`
        WHERE start_station_name="Zilker Park West"

4. How many bike rides started at "Capital Metro HQ - East 5th at Broadway" and ended at "ACC - West & 12th Street".

        SELECT
        count(trip_id)
        FROM `bigquery-public-data.austin_bikeshare.bikeshare_trips`
        WHERE 
        start_station_name="Capital Metro HQ - East 5th at Broadway" 
        and
        end_station_name="ACC - West & 12th Street"
        
5. How many bike rides started and ended at the same location? HINT: You can use a where clause and set the start location = end location.

- My answer: 184130 rides

        SELECT
        count(trip_id)
        FROM `bigquery-public-data.austin_bikeshare.bikeshare_trips`
        WHERE start_station_name=end_station_name
        
- The right answer: 159325 rides

        SELECT
        COUNT(*) AS num_rides 
        FROM `bigquery-public-data.austin_bikeshare.bikeshare_trips` 
        WHERE start_station_name = end_station_name

To count the trip_id or everything should be the same. It makes no sense that I got more rows than if I had selected everything

6. How many rides had a trip duration of exactly one hour or less?

        SELECT
        count(bikeid)
        FROM `bigquery-public-data.austin_bikeshare.bikeshare_trips`
        WHERE duration_minutes <= 60
        
7. How many bike rides had a trip duration between 1 and 2 hours (including both 1 and 2 hour trips)?

        SELECT
        COUNT(*) AS bike_rides
        FROM `bigquery-public-data.austin_bikeshare.bikeshare_trips`
        WHERE duration_minutes >= 60 and duration_minutes <= 120

8. How many bike rides were strictly greater than 3 hours?

        SELECT
        COUNT(*) AS bike_rides
        FROM `bigquery-public-data.austin_bikeshare.bikeshare_trips`
        WHERE duration_minutes > 60*3
        
9. Consider the following two types of bike rides: 
1. Started at  "ACC - West & 12th Street" and ended at "Zilker Park West"
2. Started at "Nueces @ 3rd" and ended at "Toomey Rd @ South Lamar"
Of all these types of bike rides, what was the shortest trip duration in minutes?

        SELECT duration_minutes 
        FROM `bigquery-public-data.austin_bikeshare.bikeshare_trips` 
        WHERE 
        (start_station_name = "ACC - West & 12th Street" AND end_station_name = "Zilker Park West") 
        OR 
        (start_station_name = "Nueces @ 3rd" AND end_station_name = "Toomey Rd @ South Lamar") 
        ORDER BY duration_minutes ASC LIMIT 10

10. The subscriber_type  column is a string type column. You can see all the different distinct strings in this column from this query:

SELECT
  DISTINCT subscriber_type
FROM
  `bigquery-public-data.austin_bikeshare.bikeshare_trips`

How many of these distinct strings contain the pattern "B-cycle".
You could count them manually but that is not a scalable solution.
You can answer this question using a LIKE statement.

        SELECT count(DISTINCT subscriber_type)
        FROM `bigquery-public-data.austin_bikeshare.bikeshare_trips`
        WHERE subscriber_type like "%B-cycle%"

11. Consider all the bike rides with a subscriber_type that starts with the letter "W". How many bike rides is this?

        SELECT count(subscriber_type)
        FROM `bigquery-public-data.austin_bikeshare.bikeshare_trips`
        WHERE subscriber_type like "W%"         #if must start with W, don't put % in the beginning
        
12. How many bike rides meet the following conditions all together:
        1. subscriber_type column contains the pattern string "Member"
        2. start_station_id is  3792
        3. end_station_name is  "23rd & Rio Grande"
        4. The duration is between 128 and 539 minutes (but not including 128 and 539).

        SELECT count( trip_id )
        FROM `bigquery-public-data.austin_bikeshare.bikeshare_trips`
        WHERE 
        (subscriber_type like "%Member%")
        AND
        (start_station_id=3792)
        AND
        (end_station_name="23rd & Rio Grande")
        AND
        duration_minutes > 128 and duration_minutes < 539 
        
# Quizz 4. Timestamp column

2. How many taxi trips started between 2015-12-23 and 2015-12-27? Include these two dates in your query.

        SELECT COUNT(DISTINCT unique_key) AS num_trips 
        FROM `bigquery-public-data.chicago_taxi_trips.taxi_trips` 
        WHERE trip_start_timestamp >= '2015-12-23' AND trip_start_timestamp <= '2015-12-27'

3. How many trips started in the hours of (9,10,11,12) for all the years in the dataset except for 2016? That is, exclude any trip in the year 2016 from your answer.

        SELECT COUNT(DISTINCT unique_key) AS num_trips 
        FROM `bigquery-public-data.chicago_taxi_trips.taxi_trips` 
        WHERE 
        extract(hour from trip_start_timestamp) in (9,10,11,12) 
        and
        extract(year from trip_start_timestamp) != 2016
        
4. How many trips started within the 9th hour of the day (i.e. hour = 9) on October 31st from all the years strictly before 2017?

        SELECT COUNT(DISTINCT unique_key) AS num_trips 
        FROM `bigquery-public-data.chicago_taxi_trips.taxi_trips` 
        WHERE 
        extract(hour from trip_start_timestamp) = 9 
        and 
        extract(day from trip_start_timestamp) = 31 
        and 
        extract(month from trip_start_timestamp) = 10 
        and
        extract(year from trip_start_timestamp) < 2017


5. Using only the years 2014 and 2016, how many trips started in the 16th week all together from both these two years?

        SELECT COUNT(DISTINCT unique_key) AS num_trips 
        FROM `bigquery-public-data.chicago_taxi_trips.taxi_trips` 
        WHERE extract(week from trip_start_timestamp) = 16 
        and extract(year from trip_start_timestamp) in (2014,2016)
        
# Quizz 5. 
## Starting to use all conditions

1. When counting the number of trees by health, which group within health has the least amount of trees?

Which has the most? Note: Null can be an answer.

        SELECT
          health,
          COUNT(DISTINCT tree_id) AS num_trees
        FROM
          `bigquery-public-data.new_york_trees.tree_census_2015`
        GROUP BY
          health
        ORDER BY       
          num_trees DESC
          
2. For each type of tree (use the spc_common column), calculate the average diameter (use the tree_dbh column).

How many types of trees have an average diameter strictly greater than 10?

        SELECT
          COUNT(DISTINCT spc_common),
          AVG(tree_dbh) AS media
        FROM
          `bigquery-public-data.new_york_trees.tree_census_2015`
          -- i tried to put the following here:
          --where
          --media >10
          -- nevertheless 1. it's not the position. 2. it's not hot to. always group by + having for this cases
        GROUP BY
          spc_common
        HAVING
          media >10
        ORDER BY
          media
          
3. Of all the trees that have a health status of "Poor" and a tree diameter greater than 10 ( tree_dbh > 10), how many are Damaged and how many are Not Damaged (as  measured by the sidewalk column)?

        SELECT
          sidewalk,
          COUNT(DISTINCT tree_id) AS num_trees
        FROM
          `bigquery-public-data.new_york_trees.tree_census_2015`
        WHERE
          health = 'Poor'
          AND tree_dbh > 10
        GROUP BY
          sidewalk

4. Consider the trees for which user_type is "TreesCount Staff" or "NYC Parks Staff", and spc_common is not "London planetree", and curb_loc is "OffsetFromCurb". For the trees that meet these conditions, find the maximum tree_dbh for each of the different categories/groups in the guards column.

        SELECT

        guards, MAX(tree_dbh) AS max_tree_diameter 
        FROM
          `bigquery-public-data.new_york_trees.tree_census_2015`

         where 
         user_type in( "TreesCount Staff" , "NYC Parks Staff") 
         and
         spc_common != "London planetree"
         and 
         curb_loc  = "OffsetFromCurb"
         group by 
         guards 
         
5. What is the maximum, minimum, and average tree_dbh across the entire data set?


HINT: When using  aggregate functions, you do not have to use a group by. You can just use aggregate functions in a simple select statement from the table. It will work as long as you are only returning aggregated columns and not ordering by any column.

For example (although this is rather meaningless) it will still execute:


SELECT

  SUM(tree_dbh)

FROM

  `bigquery-public-data.new_york_trees.tree_census_2015`

        SELECT
          MAX(tree_dbh) AS max_tree_diameter,
          MIN(tree_dbh ) AS min_tree_diameter,
          AVG(tree_dbh ) AS average
        FROM
          `bigquery-public-data.new_york_trees.tree_census_2015`
