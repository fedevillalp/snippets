#

## Create a New Table
Once you have instantiated your `spark context` you can use its `sql` method to pass SQL commands. In this case we use the `sql` command `CREATE TABLE IF NOT EXISTS` to create a new database << my_database >> and a new table << my_table >>. Note how we must specify the name and the type of the new columns.

```python
database_name = 'my_database'
table_name = 'my_table'

#Make sure you specify the name and Type of each column
spark.sql(f"""
CREATE TABLE IF NOT EXISTS {database_name}.{table_name}
  (column_name_1 STRING, column_name_2 STRING, column_name_3 INT)
""")

```

---
## Select Multiple Columns

```python
# Names of the columns to be selected
array_of_columns = ['column_name_1', 'column_name_2', 'column_name_n' ]

# Pass an array to the .select() method
new_data_frame = data_frame.select([x for x in array_of_columns])
```

---
## Display the Contents of Select Columns Only

```python
array_of_columns = ['column_name_1','col_name_2']
df.select(array_of_columns).show()

#Can include  a .describe() method if you want the stats for that array_of_columns
df.select(array_of_columns).describe().show()

```
## Display the Data Types of all Columns

```Python
dataframe.dtypes
```

---
## Show the Contents of a Column in a Dataframe without truncating

```python
df.show(df.count(), False)
```

---

## Get summary statistics from one specific column of a DataFrame

```python
result = df.select('column_name').describe().collect()
```

---


## Create a Histogram

```Python
# Use Spark's `histogram` function from the RDD API
number_of_bins = 5

my_histogram = (data_frame
                .select('column_name')
                .rdd
                .flatMap(lambda record: record)
                .histogram(number_of_bins)
                )

# Load the my_historgam into a Pandas DataFrame for plotting
pd.DataFrame(
    list(zip(*my_histogram)),
    columns=['bin', 'frequency']
).set_index(
    'bin'
).plot(kind='bar')


```

---

## Left Join two DataFrames

```python
result = df1.join(df2, on=['column_name_to_join'], how='left' )
```


---

## Count number of NaN's or Null's in DataFrames

```Python
from pyspark.sql.functions import isnan, when, count, col

fraction_engaged.select([count(when(isnan(x) | col(x).isNull(), x)).alias(x) for x in fraction_engaged.columns]).show()
```



## Count the number of duplicates in a DataFrames

```Python
import pyspark.sql.functions as f

df.groupBy(df.columns)\
    .count()\
    .where(f.col('count') > 1)\
    .select(f.sum('count'))\
    .show()
```


---

## Take Random Samples from a DataFrame

```python
#Limit the number of samples to 2000
N = 2000
#Specify the number of samples as a fraction of the total
fraction = 0.5

samples = df.sample(False, fraction, seed=0).limit(N)

#Double check the total
samples.count()
```

---

## Return the Mean of a Single Column

```python
from pyspark.sql.functions import mean as _mean, stddev as _stddev, col

df_stats = df.select(
    _mean(col('total')).alias('mean'),
    _stddev(col('total')).alias('std')
).collect()

mean = df_stats[0]['mean']
std = df_stats[0]['std']

print(mean, std)
```

---
## Insert Data Into Existing Table

* First make sure that your table exists. See "Create a New Table".

* Once your table exists you can proceed to insert data.

* Note that you need an active spark context.

```python
database_name = 'database_name'
table_name = 'table_name'

# Make sure your data follows the correct table structure

# Respect the data types of each column:
# In this case the table has 5 columns and we are inserting 4 new rows
# Note how Col1 and Col 4 are type INT. Col2 and Col3 and Col5 are type STRING.

spark.sql(f"""
INSERT INTO TABLE {database_name}.{table_name}
VALUES (11, 'Jon', 'Loyd', 25, 'Math'),
       (22, 'Frank', 'Mayer', 25, 'Math'),
       (33, 'Mary', 'Sten', 34, 'Chemistry'),
       (44, 'Arthur'  , 'Welmer', 38, 'Chemistry'),
"""
)
```

---

## Create a User Defined Function (UDF)
First Import `UDF` and `types`. Then define your `UDF` just like you would define a function in python. Before using a `UDF` you have to register it with Spark and specify what type it will return. In this example we are returning an `IntegerType()`. If any other type is returned it will cause an error.

Finally, to use a `UDF`, we call the method `.withColumn()` and specify a) the name of the new column where the output of the `UDF` will be deposited and b) the `UDF` to apply. The arguments of the `UDF` are the names of the existing DataFrame columns containing the data that the UDF will consume.

```python
# 1. Make sure you import udf and the correct type.
from pyspark.sql.functions import udf
# Can also use * to import ALL types instead of only IntegerType
from pyspark.sql.types import IntegerType

# 2. Create your UDF
def user_defined_function(arg_1, arg_2):
	#arg_1 will correspond to existing_col_1 and arg_2 will correspond to existing_col_2
	result = arg_1 + arg_2
	return result

# 3. Register your UDF with Spark
my_spark_UDF = udf(user_defined_function, IntegerType())

# 4. Apply your UDF to a column and specify the name of the results column
result_dataframe = data_frame.withColumn("result", my_spark_UDF('existing_col_1', 'existing_col_2'))
```

# Convert a Pyspark DataFrame to a Pandas DataFrame


```Python

pandas_df = pyspark_df.toPandas()


```

# Order a by column_name in ascending order


```Python
#Use .desc() for descending order
pyspark_df.orderBy(f.col("col_name").asc()).show(truncate=False)
```
