## **1. DROP Operations**

Remove unwanted columns or duplicate rows.

```python
`# Drop single column 
df.drop('Item_Visibility').display()  
# Drop multiple columns 
df.drop('Item_Visibility', 'Item_Type').display()  
# Drop duplicates (all columns) 
df.dropDuplicates().display()  
# Drop duplicates based on a subset 
df.dropDuplicates(subset=['Item_Type']).display()`

```
 `dropDuplicates()` and `distinct()` are almost the same —  
 `distinct()` → returns unique rows directly removing dulplicates
 `dropDuplicates()` → gives more control (can use subset).

## **2.UNION and UNION BY NAME**

Combine two DataFrames with same schema.

```python
data1 = [('1','kad'), ('2','sid')] 
schema1 = 'id STRING, name STRING' 
df1 = spark.createDataFrame(data1, schema1)  

data2 = [('3','rahul'), ('4','jas')] 
schema2 = 'id STRING, name STRING' 
df2 = spark.createDataFrame(data2, schema2) 

df1.union(df2).display()  # Simple union
df1.unionByName(df2).display() #incase column order may differ always use this one

```
## **3. STRING Functions**

```python
df.select(upper('Item_Type').alias('upper_Item_Type')).display()
#upper for all uppercase
#lower for all lower
#initcap for initial capital of each word
```
```

```

## **4. DATE Functions**

### Current Date:

```Python
from pyspark.sql.functions import current_date 
df = df.withColumn('curr_date', current_date())
# tells current date ..be sure to import function first
```

### Date Add/Subtract:

```python
from pyspark.sql.functions import date_add, date_sub 
df = df.withColumn('week_after', date_add('curr_date', 7)) 
#instead of using date_sub just use minus numbers in the date_sub function like in this case use -7
  
df.withColumn('week_before',date_add('curr_date',-7)).display()
```

### Date Difference:

```python
from pyspark.sql.functions import datediff 
df = df.withColumn('datediff', datediff('week_after', 'curr_date'))`
  

```

### Date Format:

```python
from pyspark.sql.functions import date_format 
df = df.withColumn('week_before', date_format('week_before', 'dd-MM-yyyy'))`
#using withcolumn to modify a column here
```
## **6. SPLIT, Indexing, and EXPLODE**

### SPLIT:

Split strings into arrays for the data like this 
![[Pasted image 20251111194043.png]]


```python
from pyspark.sql.functions import split df.withColumn('Outlet_Type', split('Outlet_Type', ' ')).display()
#space defines we have to devide data from there
```
### Indexing:

Access a specific array index:
``` python

df.withColumn('Outlet_Type', split('Outlet_Type', ' ')[1]).display()
#devides into 2 column
```
### EXPLODE:

Turn array values into multiple rows:

```python
from pyspark.sql.functions import explode, array_contains 
df_exp = df.withColumn('Outlet_Type', split('Outlet_Type', ' ')) df_exp.withColumn('Outlet_Type', explode('Outlet_Type')).display()  
# Check if array contains specific value
 df_exp.withColumn('Type1_flag', array_contains('Outlet_Type', 'Type1')).display()
```

## **7.GROUP BY & AGGREGATIONS**

### Example 1 – Group by one column:

```python
from pyspark.sql.functions import sum, avg df.groupBy('Item_Type').agg(sum('Item_MRP')).display()

```
### Example 2 – Group by two columns:

```python
df.groupBy('Item_Type', 'Outlet_Size').agg(sum('Item_MRP').alias("yo mama im on new column")).display()

```
### Example 3 – Multiple Aggregations:

```python
df.groupBy('Item_Type', 'Outlet_Size').agg(     sum('Item_MRP').alias('Total_MRP'),     avg('Item_MRP').alias('Avg_MRP') ).display()
```
### COLLECT_LIST (Aggregating into Arrays)**

```python
from pyspark.sql.functions import collect_list  
data = [     
('user1','book1'),     
('user1','book2'),     
('user2','book2'),     
('user2','book4'),     
('user3','book1') ] 
schema = 'user string, book string' 
df_book = spark.createDataFrame(data, schema)  df_book.groupBy('user').agg(collect_list('book')).display()
```#Useful in recommendation or tagging problems.
```
 user    	collect_list(book)
user1	["book1","book2"]
user2	["book2","book4"]
user3	["book1"]

#### PIVOT (Transpose Columns → Rows)**

```python
df.groupBy('Item_Type').pivot('Outlet_Size').agg(avg('Item_MRP')).display()
```

 Converts unique values of `Outlet_Size` into columns, with aggregated values filled in.
 ![[Pasted image 20251111202335.png]]

#### **WHEN–OTHERWISE (Conditional Logic)**

### Basic:

```python
`from pyspark.sql.functions import when, col  df = df.withColumn('veg_flag',                    when(col('Item_Type')=='Meat', 'Non-Veg').otherwise('Veg'))`
```

### Nested Conditions:

```python
`df = df.withColumn('veg_exp_flag',     when((col('veg_flag')=='Veg') & (col('Item_MRP')<100), 'Veg_Inexpensive')     
.when((col('veg_flag')=='Veg') & (col('Item_MRP')>100), 'Veg_Expensive')     
.otherwise('Non_Veg'))`
```

Acts like SQL CASE-WHEN powerful for feature engineering
### What happens internally during a `groupBy` in Spark?

**A:** Spark performs a **wide transformation** — it shuffles records with the same key (grouping column) into the same partition, so aggregations can be applied per key.

### What’s the difference between `groupBy().count()` and `countDistinct()`?

**A:**

- `groupBy().count()` → counts rows within each group.
- `countDistinct()` → counts unique combinations across all groups.
    
    
### When does aggregation trigger an action?

**A:** Aggregations are **transformations** until you call an **action** like `.show()`, `.collect()`, or `.write()`, which triggers the execution plan

|Question|Answer Summary|
|:--|:--|
|What’s the difference between `union` and `unionByName`?|`unionByName` matches columns by name (ignores order).|
|How to remove duplicates in PySpark?|Use `distinct()` or `dropDuplicates()`.|
|When does Spark perform shuffle during aggregation?|When `groupBy()` requires data from multiple partitions.|
|How is `collect_list()` useful?|Aggregates grouped values into an array for analytics.|
|What’s the use of `pivot()`?|Converts unique values in one column into separate columns.|