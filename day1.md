# Add bigquery to visual studio code:

https://github.com/google/vscode-bigquery

# Use BQ Mate Navigator

    It's a chromium addon to display how much the query costs

# In bigquery, to start working with the public dataset:

SELECT * FROM `bigquery-public-data.baseball.schedules` LIMIT 1000

# Save your dataset and pin it:

### ctrl (or alt) + click in `bigquery-public-data.baseball.schedules` to get in the left part of the screen, so you can start working with it or pin it

# Tables 

- Select a table and Schema/Details/Preview.

    Preview it's like pandas.head()

- Query table it's exactly like writing by hand the query. It just writes like a fill-the-gaps query for the table you're looking, to save time

# Chicago Crime dataset

SELECT primary_type FROM `bigquery-public-data.chicago_crime.crime` LIMIT 10