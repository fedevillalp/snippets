#

## Display ALL the rows of a Pandas DataFrame

```Python
pd.set_option('display.max_row', None)
```


---

## Display ALL the contents in the rows of a Pandas DataFrame
```Python
pd.set_option('display.max_colwidth', -1)
```



---


## Drop Null Values

#### Drop the rows in which ANY value is `null`:
```python
data_frame = data_frame.na.drop("any")
```  


---

#### Drop `null` values only if ALL values are `null` in a row:
```python
df.na.drop("all")
```  

---

## Percentage of a DataFrame


```python
df1['percentage'] = df1['Mathematics_score']/df1['Mathematics_score'].sum()
```


---

#### Round values  in data_frame

```python
x['percentage'] = (x['count']/x['count'].sum())*100
x_1 = x.sort_values(by=['round(all_dates_fraction_greater_than_zero_reqs, 2)'])
```


---

#### Drop the `null` values ONLY in the selected columns
```python
df.na.drop("all", Seq("col1", "col2", "col3"))
```

---

## Select Multiple Columns From Existing Dataframe

To select only certain columns from an existing Dataframe just pass it a list of columns

```python
list_of_columns = ['col_name_1', 'col_name_2', 'col_name_n']
df = df['list_of_columns']
```


---



## New column with value depending on multiple conditions

```python
# Create list with the conditions
conditions = [
    (df['col_name'] < 0),
    (df['col_name'] == 0),
    (df['col_name'] > 0)
    ]

# Create a list with the values corresponding to each condition
values = ['blue', 'white', 'red']

# New column 'color' whose value depends on the conditions in the list above
df['color'] = np.select(conditions, values)
```



---



##Filter DataFrame based on a column values

```python
#One condition
new_df = df[df.col_name == 'condition']

#Two Conditions
new_df = df[(df.col_name == 'condition_1') & (df.col_name == 'condition_2')]
```


## Group

```Python
#When group by needs to aggregate in this case we used .mean() but could have done other aggregations such as .sum(), count() etc...

df.groupby(['column_name']).mean()
```

## Reset index column_name

```Python
#Old indext will be added as a column
df.reset_index()

#Old index will be dropped and not added as a column
df.reset_index(drop=True)

```

## Selecting specific Rows and/or Columns of a DataFrame

```Python
#Genral structure:
#df.iloc[rows, columns]

#Get single row row_number
df.iloc[row_number]

#Get single column col_name
df.iloc[:, col_name]

#Get rows n to m
df.iloc[n:m]

#Get all rows of columns n to m
df.iloc[:, n:m]

#Get rows n to m from columns k to l
df.iloc[n:m, k:l]



```

## Group by and then count based on condition

```Python
#Group by col_name_1
#Go to col_name_2 and count the occurrence of my_item

df.groupby('col_name_1')['col_name_2'].apply(lambda x: x[x == 'my_item'].count())

```

## Filter by string contents in column of a DataFrame

```Python
# Select col_name_1 and then use str.contains to filter by string contents

result = df[df['col_name_1'].str.contains("stringcontents")]

```


## Filter by string contents in column of a DataFrame

```Python
# Pass array with columns to sort and array to specify if sort by ascending or not

df.sort_values(by=['col_name_1', 'col_name_2'], ascending=[True, False])


```

## Count NaN in a column of a Dataframe

```Python
# The .isna() will return True or False and .sum() will count the # of rows with True

df['col_name'].isna().sum()


```
