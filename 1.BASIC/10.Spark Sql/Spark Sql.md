## Create a **Temporary View**

This converts a DataFrame into a SQL table you can query.

`df.createTempView('my_view')`

✔ This creates a **session-scoped table**  
✔ Available only until cluster/session stops  
✔ Does NOT write to disk

---

## Run SQL using Magic Command (`%sql`)

This works **inside notebooks** (Databricks / Jupyter with Ipython)

```python
%sql select * from my_view where Item_Fat_Content = 'Lf'
```

✔ You can run SQL like you are in a DB  
✔ `my_view` comes from the DataFrame you registered  
✔ Output is a SQL result table

---

## Run SQL using SparkSession (`spark.sql()`)

Use this when writing PySpark code inside Python cells.

```python
df_sql = spark.sql("select * from my_view where Item_Fat_Content = 'Lf'") df_sql.display()

```
✔ Same result as `%sql`  
✔ But fully inside PySpark context  
✔ Allows chaining transformations after query

---

# **Explanations

### 🔹 What is `createTempView`?

It registers a DataFrame as a **temporary SQL table** visible in:

- `%sql` notebook cells
    
- `spark.sql()` queries
    
- Within your current Spark session
    

### 🔹 Difference Between Temp View and Global Temp View?

|Type|Scope|Usage|
|---|---|---|
|`createTempView()`|Only current session|Fast testing & development|
|`createOrReplaceGlobalTempView()`|Across all sessions|Shared data for multiple jobs|

Example global:

```python
df.createOrReplaceGlobalTempView("my_global_view") spark.sql("SELECT * FROM global_temp.my_global_view").show()
```
# **Why Use SparkSQL Instead of PySpark API?**

### Use SparkSQL when:

- You are doing BI/analytics queries
    
- You're more comfortable with SQL
    
- You want readable code for business logic
    

### Use DataFrame API when:

- Building pipelines
    
- Writing reusable transformations
    
- Need dynamic columns or programmatic logic