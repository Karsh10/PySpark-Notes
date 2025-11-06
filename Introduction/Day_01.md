# What is Apache Spark?

Apache Spark is a **distributed computing engine** for processing **large-scale data** parallely over clusters of machines 
Key points:
- In-memory processing → Faster than Hadoop
- Used for **ETL**, Streaming, ML, and SQL workloads

# What is PySpark?

Python API for Apache Spark.  
It lets us write Python code that runs on Spark clusters.

# Creating a SparkSession

Let's start with creating a new `SparkSession`. In this course, you will be usually provided with one, but creating a new one or getting an existing one is a must-have skill!

``` python
# Import SparkSession from pyspark.sql
from pyspark.sql import SparkSession
# Create my_spark
my_spark = SparkSession.builder.appName("my_spark").getOrCreate()
# Print my_spark
print(my_spark)

```
# (Exercise) Loading census data

Let's start creating your first PySpark DataFrame! The file `adult_reduced.csv` contains a grouping of adults based on a variety of demographic categories. These data have been adapted from the US Census. There are a total of 32562 groupings of adults.

We should load the csv and see the resulting schema.

```python
# Read in the CSV
census_adult = spark.read.csv("adult_reduced.csv")
# Show the DataFrame
census_adult.show()
```
![[Pasted image 20251106092121.png]]![[Pasted image 20251106092151.png]]![[Pasted image 20251106092151.png]

# How is Spark different from Pandas?

| Pandas                       | Spark                 |
| ---------------------------- | --------------------- |
| Runs on 1 machine            | Runs on many machines |
| Small data                   | Big data              |
| Can crash with memory issues | Auto scaling          |

# What is lazy evaluation in Spark?

Spark waits to execute transformations until an **action** like `.show()` or `.count()` is wrote.

# Spark cluster

A key component of working with PySpark is clusters. A Spark cluster is a group of computers (nodes) that collaboratively process large datasets using Apache Spark, with a master node coordinating multiple worker nodes.

      
