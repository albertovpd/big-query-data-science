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
    
# SELECT __ as __ 

    You can always give an alias to a column/table/whatever

# SELECT:

- *   -   all
- count(*)    -    numbers of records on the table
- count(DISTINCT <column>)  -   print how many uniques
- distinct <column> -   like pd.value_counts(). shows just the uniques
- distinct <column1>, <column2>...  -   deliver all possible combinations of columns and returning it in a distinct list
- 
    
-------------------------

   

# FROM

        table/whatever

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
        
- Looking for some text or word

        WHERE 
        <column> like " %word/text you're looking for%"   #CASE SENSITIVE
        
        "word%" =   it must start with "word" and it will show all the following sentence
        "%word" =   the other way around
        so doble "%__%" will take everything
        
    If you don't know what you're looking for is capital or lower, just use lower to everything
    
        WHERE
        lower(<column> like '%word%')
        


..
# LIMIT

- limit the number of shown records

# ORDER BY

- <column1> ASC / DESC, <column2> ASC/DESC  ...
-  


# Chicago Crime dataset

SELECT primary_type FROM `bigquery-public-data.chicago_crime.crime` LIMIT 10
