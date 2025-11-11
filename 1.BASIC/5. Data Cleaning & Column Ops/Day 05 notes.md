## Data Manipulation with DataFrames

In PySpark, data manipulation means:
- Cleaning null or incorrect values  
- Modifying column structure (add, rename, drop)  
- Filtering and grouping data 
It’s how we *prepare raw data* for analysis and pipelines.

## Handling Missing Data

Missing or null values can break your transformations or skew results.  
PySpark provides multiple ways to handle them.

#### Drop Nulls

``` python
df.na.drop().show()                      
# Drop any row with at least one null
df_filtered = df.filter(col('ColumnName').isNotNull())    
# Drop only if 'salary' column is null
```
#### Fill Nulls

``` python
df.na.fill({"salary": 0, "department": "Unknown"}).show()
```

Use `.na.fill()` when you can replace missing values with defaults (to avoid data loss)

## Column Operations 

``` python
from pyspark.sql.functions import col, lit
#col is used a refer a column inside expression 
#lit is used to insert constant values

df = df.withColumn("salary_in_lakhs", col("salary") / 100000)
df.withColumn("country", lit("India"))          # adds a constant column
```

#### Rename Column

``` python
`df = df.withColumnRenamed("dept", "department")`
```

Useful for clarity and consistency when combining datasets.

#### Drop Columns

```python
`df = df.drop("temp_col", "extra_info")`
```

Removes unnecessary columns to optimize memory and focus analysis.

## Row Operations

#### Filtering Rows

```python
df.filter(df.salary > 50000).show() 
df.filter(df.city == "Delhi").show()
```
####  Grouping Rows
```python

from pyspark.sql.functions import avg, count  df.groupBy("department").agg(     avg("salary").alias("avg_salary"),     count("*").alias("num_employees") ).show()
```
