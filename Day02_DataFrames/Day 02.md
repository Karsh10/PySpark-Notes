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
