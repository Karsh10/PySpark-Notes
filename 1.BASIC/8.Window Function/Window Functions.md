## **What Are Window Functions?**

Window functions perform calculations across **groups of rows** without collapsing them into a single value like `groupBy()` does.

## Ranking Functions
### 1)**ROW_NUMBER()**

Gives a unique, sequential number to each row
```python
df.withColumn("row_num", row_number().over(window.orderby('Item_Identifier')))
```
### 2)**RANK()**

Gives rank with **gaps** in numbering when ties occur

```python
df.withColumn("rank", rank().over(window.orderby('Item_Identifier'))).display()
```

### 3)**DENSE_RANK()**

Same as rank but **no gaps** in numbering.

```python
df.withColumn("denserank", dense_rank().over(window.orderby('Item_identifier'))) df_dense.show()`
```
## Aggregation over Windows (AVG, SUM, etc.)
#### Cumulative Sum (prefer data with bara sql lecture for understanding the topic in detail)

```python
df.withColumn('cumsum',sum('Item_MRP').over(Window.orderBy('Item_Type'))).display()

df.withColumn('cumsum',sum('Item_MRP').over(Window.orderBy('Item_Type').rowsBetween(Window.unboundedPreceding,Window.currentRow))).display()
```
## **LAG and LEAD Functions**

Used to access previous or next row values.

### a) LAG()

Compare current salary to previous salary in the same department.
```python

`df_lag = df.withColumn("prev_salary", lag("salary", 1).over(windowSpec)) df_lag.show()`

```
### b) LEAD()

Look ahead to next salary.

```python
`df_lead = df.withColumn("next_salary", lead("salary", 1).over(windowSpec)) df_lead.show()`
```

| Keyword                     | Meaning          |
| --------------------------- | ---------------- |
| `Window.currentRow`         | Current row only |
| `Window.unboundedPreceding` | From first row   |
| `Window.unboundedFollowing` | Till last row    |
| `rowsBetween(a,b)`          | Custom range     |
### USER DEFINED FUNCTIONS (UDF)
Creating own functions

#### STEP - 1

```python
def my_func(x): 
return x*x
```

#### STEP - 2

```python
my_udf = udf(my_func)

df.withColumn('mynewcol',my_udf('Item_MRP')).display()
```

## **Mini Project – Top 2 Salaries Per Department**

```python
`windowSpec = Window.partitionBy("dept").orderBy(col("salary").desc())  
df_top2 = df.withColumn("rank", rank().over(windowSpec)) \             .filter(col("rank") <= 2)  df_top2.show()`
```