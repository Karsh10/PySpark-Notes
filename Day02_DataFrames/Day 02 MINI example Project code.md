```python
from pyspark.sql import Sparksession
spark= Sparksession.builder.appname("Minimal Example").getOrCreate()
df=spark.read.csv("Example02,csv",header=True,inferSchema=True)
filtered_df=df.filter(df.salary>50000)
grouped_df=filtered_df.groupBy("department")
avgsalary_df=grouped_df.avg("salary")
avgsalary_df.show()
```
