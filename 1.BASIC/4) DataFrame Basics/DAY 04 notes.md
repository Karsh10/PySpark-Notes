## What Are Spark DataFrames?

A **DataFrame** is a distributed table with rows and columns....similar to a SQL table,but runs across multiple machines.  
PySpark DataFrames are immutable and optimized for parallel processing

## Creating DataFrames from Various Data Sources

PySpark supports multiple file formats — each with its own pros and cons:

| Format      | Pros                                | Cons                         | Load Function          |
| ----------- | ----------------------------------- | ---------------------------- | ---------------------- |
| **CSV**     | Simple, human-readable, widely used | No schema enforcement        | `spark.read.csv()`     |
| **JSON**    | Handles nested / hierarchical data  | Storage-heavy on large scale | `spark.read.json()`    |
| **Parquet** | Schema-enforced, highly efficient   | Slightly complex             | `spark.read.parquet()` |

## Creating DataFrames from Files

`df = spark.read.csv("employees.csv", header=True, inferSchema=True)`

|Parameter|Meaning|
|---|---|
|`header=True`|Use first row as column names|
|`inferSchema=True`|Auto-detect data types|
``` python
data = [`
    `("Utkarsh", "India", 20),`
    `("Karsh", "USA", 22),`
    `("Akash", "India", 26)`
`]`

`columns = ["name", "country", "age"]`

`df = spark.createDataFrame(data, columns)`
`df.show()`
`df.printSchema()

```

## Viewing the Schema

``` python
df.printSchema()
```

Shows column names, data types

## Functions for PySpark Analytics

|Function|Purpose|SQL Equivalent|
|---|---|---|
|`select()`|Pick specific columns|`SELECT col1, col2`|
|`filter()` / `where()`|Filter rows based on conditions|`WHERE`|
|`groupBy()`|Group rows by column(s)|`GROUP BY`|
|`agg()`|Apply aggregation functions like `sum()`, `avg()`|`SUM(), AVG()`|
#### Filtering and Selecting

```python
df.filter(df.age > 50).select("name", "age").show()
```

Filters employees older than 50 and displaying only `name` and `age`

## Reading a CSV and performing aggregations

``` python
# Load the CSV file into a DataFrame
salaries_df = spark.read.csv("salaries.csv", header=True, inferSchema=True)
# Count the total number of rows
row_count = salaries_df.count()
print(f"Total rows: {row_count}")
# Group by company size and calculate the average of salaries
salaries_df.groupBy("company_size").agg({"salary_in_usd": "avg"}).show()
salaries_df.show()
```

### Filtering by company

Using that same dataset from the last exercise, you realized that you only care about the jobs that are entry level (`"EN"`) in Canada (`"CA"`). What does the salaries look like there? Remember, there's already a `SparkSession` called `spark` in your workspace
# Average salary for entry level in Canada

``` python 
CA_jobs = 
ca_salaries_df.filter(ca_salaries_df['company_location'] == "CA").filter(ca_salaries_df['experience_level']
== "EN").groupBy().avg("salary_in_usd")
# Show the result
CA_jobs.show()
```

## Schema inference and manual schema definition

Spark can automatically infer schemas...but sometimes it misinterprets data types, particularly with complex or ambiguous data. Manually 
defining a schema can ensure accurate data handling.
Manual schemas prevent Spark from guessing wrong types (e.g., reading numbers as strings.

## DataTypes in PySpark DataFrames

|Type|Example|Description|
|---|---|---|
|`IntegerType()`|42|Whole numbers|
|`LongType()`|100000|Large integers|
|`FloatType()`|3.14|Low-precision decimals|
|`DoubleType()`|12.567|High-precision decimals|
|`StringType()`|"Utkarsh"|Text values|

All come from pyspark.sql.types

## DataType Syntax Recap

``` python
from pyspark.sql.types import StructType, StructField, StringType, IntegerType  
schema = StructType([ StructField("emp_id", IntegerType(), True), StructField("emp_name", StringType(), True),    
StructField("salary", DoubleType(), True) ] )
#set the Schema 
df= spark.createdataframe(data,schema= schema)

```


`StructType()` → whole schema  
`StructField()` → each column definition

## Data Frames Selection and Filtering 
- Use .select() to choose specific columns 
- Use .filter () or .where() to filter data 
- Use .Sort to order by a collection of columns 

``` python
df.select("name","salary").filter(df.salary > 50000).show()
```
## Sorting & Dropping Missing Values

### Sorting

``` python
df.sort(df.age).show() 
df.orderBy(df.salary.desc()).show()
```

- `.sort()` → simple sorting
    
- `.orderBy()` → multi-column or ascending/descending control

### Handling Nulls

``` python
df.na.drop().show()                          
# Drop rows with any nulls         
df.na.fill({"city":"Unknown","salary":0}).show()  
# Replace missing values
```

✔ `.na.drop()` removes nulls  
✔ `.na.fill()` fills or imputes missing data

# Q) Schema writeout

We've loaded Schemas multiple ways now. So lets define a schema directly. We'll use a Data dictionary:

|Variable|Description|
|---|---|
|age|Individual age|
|education_num|Education by degree|
|marital_status|Marital status|
|occupation|Occupation|
|income|Categorical income|
```python
from pyspark.sql.types import StructType, StructField, IntegerType, StringType
# Fill in the schema with the columns you need from the exercise instructions
schema = StructType([StructField("age",IntegerType()),

                     StructField("education_num",IntegerType()),

                     StructField("marital_status",StringType()),

                     StructField("occupation",StringType()),

                     StructField("income",StringType())

                    ])
# Read in the CSV, using the schema you defined above

census_adult = spark.read.csv("adult_reduced_100.csv", sep=',', 
header=False, schema=schema)
# Print out the schema
census_adult.printSchema()
```
