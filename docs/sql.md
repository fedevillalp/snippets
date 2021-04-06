#

## Calculate difference in dates (HIVE SQL)

```python
datediff(
         to_date(String column_with_timestamp_1),
         to_date(String column_with_timestamp_2)
         )
```

---


## List all columns in a table (SQL)
``` python
DESCRIBE name_of_table;
```


---

## IF clause (SQL)

```python
IF(col_name IS NULL, 'Missing', col_name ) AS new_col_name,
```


---


## CASE clause (SQL)

```Python
CASE
    WHEN column_name_1 = 'condition_1' THEN 'result_1'
    WHEN column_name_2 LIKE 'begins_with_some_text%' THEN 'result_2'
    WHEN column_name_3 LIKE 'begins_with_some_text%' THEN 'result_3'
    ELSE 'Other'
    END AS new_column_name,
```



## Add a new column with a String

```python
cast('my_string' as VARCHAR) AS new_column_name
```

## WITH keyword

```Python
#One query and select columns
WITH query_name (column_name1, ...) AS
     (SELECT ...)

SELECT ...

#Multiple queries
WITH query_name1 AS (
     SELECT ...
     )
   , query_name2 AS (
     SELECT ...
       FROM query_name1
        ...
     )
SELECT ...
```
