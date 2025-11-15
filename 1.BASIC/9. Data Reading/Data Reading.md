## Writing to CSV
```python
df.write.format('csv')\
  .save('/FileStore/tables/CSV/data.csv')
```
## Writing to JSON

```python
df.write.mode("overwrite").json("output/day9_json/")
```
## Modes 

| Mode                      | Description                  | Use Case                       |     |
| ------------------------- | ---------------------------- | ------------------------------ | --- |
| `overwrite`               | Replaces existing files      | Safe when regenerating results |     |
| `append`                  | Adds to existing data        | Incremental ETL                |     |
| `ignore`                  | Skips writing if file exists | Safe for re-runs               |     |
| `error` / `errorifexists` | Throws exception if exists   | For critical jobs              |     |
#### APPEND

```python
df.write.format('csv')\ 
.mode('append')\ 
.save('/FileStore/tables/CSV/data.csv')
```

#### Overwrite

```python
df.write.format('csv')\ 
.mode('overwrite')\ 
.save('path','/FileStore/tables/CSV/data.csv')
```

#### Error

```python
df.write.format('csv')\
.mode('error')\ 
.save('path','/FileStore/tables/CSV/data.csv')
```
#### Ignore
```python

df.write.format('csv')\
.mode('ignore')\
.option('path','/FileStore/tables/CSV/data.csv')\ # alt method
.save()
```

#### writing to PARQUET (COLUMNAR storage)

```python
df.write.format('parquet')\
.mode('overwrite')\
.option('path','/FileStore/tables/CSV/data.csv')\
.save()
```
**Why Parquet?**(BEST)

- Columnar storage → faster reads for selective columns
    
- Compression → smaller file sizes
    
- Schema evolution support
    

✅ Spark reads and writes Parquet by default in most production setups.
#### TABLE
```python

df.write.format('parquet')\
.mode('overwrite')\ 
.saveAsTable('my_table')

df.display()

```
## Managed vs External Tables

| Type     | Data Location                      | Who Owns Data | When Dropped?     |
| -------- | ---------------------------------- | ------------- | ----------------- |
| Managed  | Inside Spark’s warehouse directory | Spark         | Data is deleted   |
| External | Custom path provided               | You           | Data is preserved |

## **Reading Data Back**
```python 

df_parquet = spark.read.parquet("output/day9_parquet/") df_parquet.show()  
df_json = spark.read.json("output/day9_json/") 
df_json.show()
```

## **Save to Delta Format (Preview for Day 11)**

If Delta Lake is configured in your Spark:

```python
df.write.format("delta")
.mode("overwrite")
.save("output/day9_delta/")
```

Delta adds ACID transactions, versioning, and schema enforcement.