## **What Are Joins?**

Joins combine rows from two DataFrames based on a **key column**

## **Types of Joins in PySpark**

| Type    | Description                                  | SQL Equivalent       |
| ------- | -------------------------------------------- | -------------------- |
| `inner` | Returns only matching records                | `INNER JOIN`         |
| `left`  | All from left + matched from right           | `LEFT OUTER JOIN`    |
| `right` | All from right + matched from left           | `RIGHT OUTER JOIN`   |
| `anti`  | Only rows from left that _don’t_ match right | `NOT IN` filter-like |
## JOINS

```python
`dataj1 = [('1','gaur','d01'), 
('2','kit','d02'), 
('3','sam','d03'), 
('4','tim','d03'), 
('5','aman','d05'), 
('6','nad','d06')] 
schemaj1 = 'emp_id STRING, emp_name STRING, dept_id STRING' 
df1 = spark.createDataFrame(dataj1,schemaj1) 
dataj2 = [('d01','HR'),
 ('d02','Marketing'), 
 ('d03','Accounts'), 
 ('d04','IT'), 
 ('d05','Finance')] 
 schemaj2 = 'dept_id STRING, department STRING' df2 = spark.createDataFrame(dataj2,schemaj2)`

```

#### Inner Join
Returns records that  only have matching keys in both DataFrames.

```python
df1.join(df2, df1['dept_id']==df2['dept_id'],'inner').display()
```
|emp_id|emp_name|dept_id|dept_id|department|
|---|---|---|---|---|
|1|gaur|d01|d01|HR|
|2|kit|d02|d02|Marketing|
|3|sam|d03|d03|Accounts|
|4|tim|d03|d03|Accounts|
|5|aman|d05|d05|Finance| 

#### Left JOIN
All records from left + matching ones from right
**Unmatched rows on the right** get nulls.

```python
df1.join(df2,df1['dept_id']==df2['dept_id'],'left').display()
```

|emp_id|emp_name|dept_id|dept_id|department|
|---|---|---|---|---|
|1|gaur|d01|d01|HR|
|2|kit|d02|d02|Marketing|
|3|sam|d03|d03|Accounts|
|4|tim|d03|d03|Accounts|
|5|aman|d05|d05|Finance|
|6|nad|d06|null|null|


#### Right JOIN
All from right + matching ones from left.
```python
df1.join(df2,df1['dept_id']==df2['dept_id'],'right').display()
```

|emp_id|emp_name|dept_id|dept_id|department|
|---|---|---|---|---|
|1|gaur|d01|d01|HR|
|2|kit|d02|d02|Marketing|
|4|tim|d03|d03|Accounts|
|3|sam|d03|d03|Accounts|
|null|null|null|d04|IT|
|5|aman|d05|d05|Finance|
#### ANTI JOIN
returns rows in left that _don’t_ exist in right.
```python

df1.join(df2,df1['dept_id']==df2['dept_id'],'anti').display()

```
|emp_id|emp_name|dept_id|
|---|---|---|
|6|nad|d06| 
