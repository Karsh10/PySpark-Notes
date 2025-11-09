## 1)Loading and fixing schema 
```python
from pyspark.sql import sparksession
from pyspark.sql.functions import col

spark= sparksession.build.appname("Cleanup").getorcreate()
df=spark.read.csv("Cleanup.csv",header=true,inferschema=true)

```
## 2) Renaming columns,Making a column and filling missing values 

```python
from pyspark.sql.functions import col, lit, avg
df= df.withcolumnrenamed("SalAry","Salary)
df=df.withcolumn("Salary_inlakhs",col("Salary")/100000)
df=df.na.fill({"Department" : Unknown,"Salary" : 0})


```
## 3) Removing 0 salaries 

```python
df_removing= df.filter(df.Salary>0)

```

## 4) the average of each departments

```python
avg_salary= df_removing.groupBy("Department").agg(avg("Salary")
.orderBy(col("avg_salary").desc()))

```

# Whole Code 
```Python
## 1)Loading and fixing schema 
from pyspark.sql import SparkSession
from pyspark.sql.functions import col

spark= SparkSession.build.appname("Cleanup").getorcreate()
df=spark.read.csv("Cleanup.csv",header=True,InferSchema=True)

#2)Renaming columns,Making a column and filling missing values 


from pyspark.sql.functions import col, lit, avg
df= df.WithColumnRenamed("SalAry","Salary)
df=df.withColumn("Salary_inlakhs",col("Salary")/100000)
df=df.na.fill({"Department" : "Unknown" ,"Salary" : 0})

# 3) Removing 0 salaries 

df_removing= df.filter(df.Salary>0)

# 4) the average of each departments


avg_salary= df_removing.groupBy("Department").agg(avg("Salary").alias("avg_salary").orderBy(col("avg_salary").desc()))

```