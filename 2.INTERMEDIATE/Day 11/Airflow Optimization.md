# **STEP 1 — Turn OFF AQE**

```python
spark.conf.set("spark.sql.adaptive.enabled", "false") spark.conf.get("spark.sql.adaptive.enabled")
```
### **You cannot disable AQE in Databricks Free Community Edition.**
 Because Community Edition runs on **serverless compute**

**Ignores** critical performance configs like
    
    `spark.sql.adaptive.enabled`
        
    `spark.sql.shuffle.partitions`
        
    `spark.sql.files.maxPartitionBytes`
        
    `spark.sql.autoBroadcastJoinThreshold`
        
Does **not** allow cluster-level config editing
Does **not** allow custom SparkSession creation
Does **not** allow RDD operations (`df.rdd` → blocked)
What this means- 
AQE (Adaptive Query Execution) **changes the number of partitions automatically** based on data size.
Because if AQE is ON → Spark adjusts partitions → you cannot learn partition basics.

So:
- AQE OFF = manual control
    
- AQE ON = automatic optimization (you will learn later)
    

---

#  STEP 2 — Read the CSV File**

```python
df = spark.read.format("csv")\         
.option("inferSchema",True)\         
.option("header",True)\         
.load("/Volumes/workspace/spark_optimization/learning/BigMart Sales (1).csv")
```

### Explanation:

- `inferSchema=True` → Spark looks at values and guesses types (int, string, double)
- `header=True` → First row is column names
- `load()` → read CSV file

This is your **base DataFrame**.


#  **STEP 3 — Chek Number of Partitions**

```python
df.rdd.getNumPartitions()
#doesnt work in free editon..need clusters
```
### What this does:

Shows how many **boxes** (partitions) Spark created to store your data.

Default is usually **some number like 1, 2 or 4** (depends on file size).

Small data = few partitions  
Large data = many partitions

---

# **STEP 4 — Change Default Partition Size**

```python
spark.conf.set("spark.sql.files.maxPartitionBytes", 131072)
#doesnt work in free editon..need clusters
```

Spark uses `maxPartitionBytes` to decide:

> **How much data (in bytes) can fit inside 1 partition**

Your teacher set:

- **131072 bytes = 128 KB**
- **134217728 bytes = 128 MB**

### Why did he do this?

To SHOW you:

- If partition size is **tiny (128 KB)** → Spark will create MANY partitions
- If partition size is **big (128 MB)** → Spark will create FEWER partitions

You are NOT supposed to use 128KB in real life —  
it is ONLY for demonstration.

### REAL VALUE = 128 MB

That's what companies use.

# **STEP 5 — Repartition Data**

```python
df = df.repartition(10) 
df.rdd.getNumPartitions()
```

### Meaning:

> “Spark, shuffle the entire dataset and create **exactly 10 equal partitions**.

This forces Spark to:

- Shuffle data
- Redistribute rows
- Balance boxes

Use this before:

- Join on a key
- Large groupBy

#  **STEP 6 — See Which Row Belongs to Which Partition**

```python
df = df.withColumn("partition_id", spark_partition_id()) 
df.display()

```
### This shows you:

- partition 0
- partition 1
- partition 2
- … partition 9

Now you can SEE how Spark split your data.

This is SUPER USEFUL for learning partition behavior.

---

# **STEP 7 — Write Data**

```python
df.write
.format("parquet")\     
.mode("append")\     # to overwrite write overwrite
.option("path", "dbfs:/Volumes/workspace/spark_optimization/learning/parquetWrite")\     
.save()
```

### Meaning:

Spark will write:

- In parquet format
- Create **1 file per partition**
- Put files inside path: `/parquetWrite`

Since you had **10 partitions**, you will see **10 parquet files**.
you cant read a directory containing both Parquet and CSV files using the Parquet reader, which causes the error. You must ensure only Parquet files are read, or use the ignoreCorruptFiles option to skip non-Parquet files.

---

# **STEP 8 — Read New Parquet + Filter**

```python
df_new = spark.read.format("parquet")\               
    .load("/Volumes/workspace/spark_optimization/learning/parquetWrite")  
df_new = df_new.filter(col("Outlet_Location_Type") == 'Tier 1') 
df_new.display()
```

### Without Partitioning:

Spark reads **ALL files**, then applies filter.

This is slow when data is BIG.

---

# **STEP 9 — Write Data WITH Partitioning**

```python
df.write
.format("parquet")\       
.mode("append")\       
.partitionBy("Outlet_Location_Type")\       .option("path","/Volumes/workspace/spark_optimization/learning/parquetWriteOpt")\       
.save()
```

### Meaning:

Spark will create a folder structure:

```
parquetWriteOpt/    
Outlet_Location_Type=Tier 1/     
Outlet_Location_Type=Tier 2/     
Outlet_Location_Type=Tier 3/
```

Inside each folder → parquet files belong to THAT category.


---

## **STEP 10 — Read New Optimized Data**

```python
df_new = spark.read.format("parquet")\               .load("/Volumes/workspace/spark_optimization/learning/parquetWriteOpt")  
df_new = df_new.filter(col("Outlet_Location_Type") == 'Tier 1') df_new.display()
```

# Questions -

### Q1: Difference between repartition and coalesce?
**Repartition** → shuffle, increase or decrease  
**Coalesce** → no shuffle, only decrease

### Q2: What is partition pruning?
Spark reads only the folder matching your filter.

### Q3: Why 128MB partition size?
Matches HDFS block size → fastest for distributed read.

### Q4: Why is filtering faster on partitioned data?
Only reads relevant partitions (folders).

### Q5: What is spark_partition_id()?
Shows which row belongs to which partition.