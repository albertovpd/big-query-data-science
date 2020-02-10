# Add bigquery to visual studio code:

https://github.com/google/vscode-bigquery

# Use BQ Mate Navigator

    It's a chromium addon to display how much the query costs

# In bigquery, to start working with the public dataset:

SELECT * FROM `bigquery-public-data.baseball.schedules` LIMIT 1000

# Save your dataset and pin it:

### ctrl  + click in `bigquery-public-data.baseball.schedules` to get in the left part of the screen, so you can start working with it or pin it

# Tables 

- Select a table and Schema/Details/Preview.

    Preview it's like pandas.head()

- Query table it's exactly like writing by hand the query. It just writes like a fill-the-gaps query for the table you're looking, to save time

-----------------------
# Order:

- SELECT
- FROM
- WHERE
- GROUP BY
- (just if group by was done) HAVING -- <>= --
- ORDER BY
- LIMIT

        SELECT
          contributing_factor_vehicle_1,
          COUNT(*) AS num_accidents
        FROM
          `bigquery-public-data.new_york_mv_collisions.nypd_mv_collisions`
        WHERE
          contributing_factor_vehicle_1 != 'Unspecified'
        GROUP BY
          contributing_factor_vehicle_1
        HAVING
          num_accidents > 1000
        ORDER BY
          num_accidents DESC
        LIMIT
          1000
          
# SELECT __ as __ 

    You can always give an alias to a column/table/whatever

# SELECT:

- *   -   all
- count(*)    -    numbers of records on the table
- count(DISTINCT <column>)  -   print how many uniques
- distinct <column> -   like pd.value_counts(). shows just the uniques
- distinct <column1>, <column2>...  -   deliver all possible combinations of columns and returning it in a distinct list
- SUM
- AVG
- MAX / MIX ... 
- ROUND( ,n)    round to n the decimals
- concat(cast(<t1.column1> as __), )    :   to glue columns or modify rows  

### https://cloud.google.com/bigquery/docs/reference

-------------------------
# cast / extract
casting :   it's like a lambda apply. con convert columns and more stuff. useful when you change the type of a column 
DATE:   we need to cast the date like in the example

extract = get some info that it's with more information in a row (it's like a pandas explode, or pd.split and create new columns, or an unwind in mongodb). it's cool because extract create new columns with the wanted information

        select 
        start_time as start_time_timestamp,
        cast(start_time as date) as start_time_date,
        extract (hour from start_time) as start_time_hour,
        extract (day from start_time) as start_time_day,
        extract (year from start_time) as start_time_year,
        extract (month from start_time) as start_time_month,
        extract(week from start_time) as start_time_week
        FROM
        `bigquery-public-data.austin_bikeshare.bikeshare_trips` 
        WHERE start_time > '2018-10-01'  #date format is type(str) #maybe you need to cast(start_time as date)
        LIMIT
        100
        
We can also do:

        where extract (hour from start_time) IN (17,18, 19, 20)
        

        
(we can do this because is UTC format)

# CASE
    case
        when condition1 then result1
        when condition2 then result2
        etc...
        else result end as ...

# FROM

# JOIN

You can join all tables you want

- left join :   take all from the left table and select the common of the right table. FROM <table1> LEFT JOIN <table2>... so the left one is the 1st
- right join    :   goddamn the same
- inner join    :   commons to both
- you must put an alias to the table you're using like the following: (the table alias are "m" and "a", without writing "as" or whatever)

        SELECT
        m.country_name,
        -- you can't do  "a.country_name" because country name is already taken from "m"
        m.year
        FROM
        `bigquery-public-data.census_bureau_international.midyear_population` m
        LEFT JOIN
        `bigquery-public-data.census_bureau_international.country_names_area` a
        ON
        --m.country_code = a.country_code
        --to make sure there's match between tables:

        trim(lower(m.country_code)) = trim(lower(a.country_code))
        ORDER BY
        year,
        country

    - You need to be careful because when joining tables you create duplicates
    - Maybe you need to lower columns to get a match, or in ON, put everything there like lower(cosa de 1 tabla)=lower(cosa de 2 tabla)




# WHERE (aditional conditions)

- Conditions

    - IMPORTANT! when equaling a condition to a str type, the str must be between "__"
    - conditions do not have == like in Python. Just 1 =

        select 
        *
        from `bigquery-public-data.usa_names.usa_1910_current` 

        where 
          ( state="FL" and year>2000 AND gender="M" )
          OR
          ( state="NY" and year>1000)
        limit 
        100 

- in (= or)

        select
        ___
        from ____
        WHERE <column> in (a,b)   #a,b parameters
        (exactly like <column> = a or <column> = b
        
- is null / is not null

    CUIDAO! 
    SELECT count(*) :   counts everything
            count(<column>):    count just not nulls
    
    So, if you write:
    
        SELECT 
        count(*) AS count1,
        count(dropoff_location) AS count2
        FROM
        `bigquery-public-data.chicago_taxi_trips.taxi_trips` 
        WHERE dropoff_location is null
    
    The output is:
        count1: a lot
        count2: 0
        
    Nevertheless if we write:
    
        WHERE dropoff_location is null
        
    Then both columns has the same lenght
    


- Looking for some text or word

        WHERE 
        <column> like " %word/text you're looking for%"   #CASE SENSITIVE
        
        "word%" =   it must start with "word" and it will show all the following sentence
        "%word" =   the other way around
        so doble "%__%" will take everything
        
    If you don't know what you're looking for is capital or lower, just use lower to everything
    
        WHERE
        lower(<column> like '%word%')
        
# GROUP BY
- you can't do a "group by" if the rest of the rows have more than 1 element. check always taht
- like in pandas, but pandaless and pandafunless
- you only use HAVING when using a GROUP BY

        select 
            name, sum(number) as sum
        from `bigquery-public-data.usa_names.usa_1910_2013` 
        where
            gender = "F"
        group by
            name
        having
            num_people >1000000
        limit 1000
        
it is not that easy to group by with more than 1 column (not like pandas), it needs to be consequent with the rest of columns

# GROUP BY multiple columns
 
https://www.udemy.com/course/sql-for-data-science-with-google-big-query/learn/lecture/13081162#overview


        SELECT
            column1, column2
        FROM
            TABLE
        GROUP BY
            column1, column2
            

## Group by example:

        SELECT major_category  FROM `bigquery-public-data.london_crime.crime_by_lsoa` 

        LIMIT 1000

Will give you a list of all crimes. Performing

        SELECT major_category  FROM `bigquery-public-data.london_crime.crime_by_lsoa` 
        group by major_category 

        LIMIT 1000
        
You'll get the uniques, but not how manu pf them. If summing values:

        SELECT major_category, sum(value) as num_crimes  
        FROM `bigquery-public-data.london_crime.crime_by_lsoa`
        group by major_category 

        LIMIT 1000
        
it sums values. kind of weird because there is no column with "value".Value it's like saying "eh, all that, sum up!"
..----------------

# LIMIT

- limit the number of shown records

# ORDER BY

- <column1> ASC / DESC, <column2> ASC/DESC  ...
  


# Chicago Crime dataset

SELECT primary_type FROM `bigquery-public-data.chicago_crime.crime` LIMIT 10
sudo 