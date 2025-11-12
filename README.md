# ⚡⚡Spark + PySpark Mastery ⚡⚡

## Overview 🧭 
A personal log tracking my 30-day Spark + PySpark challenge.  
This is *my personal understanding, notes, and reflections* as I move from **foundations → optimization → streaming → projects** 
Star it If it looks helpful


---

## Goals 🎯 
- Understand Spark’s internals and DAG execution  
- Master DataFrames, SQL, and Transformations  
- Learn optimizations: caching, AQE, skew handling  
- Implement Structured Streaming and Delta Lake pipelines  
- Document every phase for recall and interviews  

---

## 🟢 BASIC LEVEL (Days 1–10)
> *Foundation — Understanding Spark’s core concepts and DataFrame basics.*

| Day | Topic | Content Covered |
|:--:|:--|:--|
| 1 | [Spark Architecture + Lazy Evaluation](https://github.com/Karsh10/PySpark-Notes/blob/4496cd834ce4c69f32cbf5620e48b3c7261ad35c/1.BASIC/1.%20Introduction/DAY%2001.md) | Spark driver, executors, DAG, lazy vs eager execution |
| 2 | RDDs & Transformations | RDD concepts, map/filter/flatMap, narrow vs wide transformations |
| 3 | Spark UI + Jobs & Stages | Understanding Spark UI, job → stage → task breakdown |
| 4 | [DataFrame Basics](https://github.com/Karsh10/PySpark-Notes/blob/4496cd834ce4c69f32cbf5620e48b3c7261ad35c/1.BASIC/4.%20DataFrame%20Basics/DAY%2004%20notes.md) | Creating DataFrames, reading CSV/JSON, schema inference |
| 5 | [Data Cleaning & Column Operations](https://github.com/Karsh10/PySpark-Notes/blob/4496cd834ce4c69f32cbf5620e48b3c7261ad35c/1.BASIC/5.%20Data%20Cleaning%20%26%20Column%20Ops/Day%2005%20notes.md) | Handling nulls, `withColumn`, `na.fill`, `drop`, renaming columns |
| 6 | [Aggregations & GroupBy](https://github.com/Karsh10/PySpark-Notes/blob/4496cd834ce4c69f32cbf5620e48b3c7261ad35c/1.BASIC/6.%20Agg%20Functions%20%2CGroup%20By%20etc/PySpark%20Aggregations%20and%20Functions.md) | `groupBy`, `agg`, `count`, `sum`, `avg`, aliasing |
| 7 | [Joins in PySpark](https://github.com/Karsh10/PySpark-Notes/blob/0d7d96a98e3bd71ec2d586468359db70ea4fa9e9/1.BASIC/7.%20Joins/Joins.md)| Inner, left, right, full joins, and broadcast joins |
| 8 | SparkSQL | `createOrReplaceTempView`, running SQL queries, comparing plans |
| 9 | Window Functions | Ranking functions (`rank`, `dense_rank`, `row_number`), partitions |
| 10 | Data Writing & File Formats | Write to CSV, JSON, Parquet, use of partitionBy and modes |

## 🟡 INTERMEDIATE LEVEL (Days 11–20)
> *Performance optimization, tuning, caching, and Delta Lake fundamentals.*

| Day | Topic | Content Covered |
|:--:|:--|:--|
| 11 | Partitioning Optimization | `repartition`, `coalesce`, partition sizing and performance |
| 12 | Join Optimization | Broadcast join, sort-merge join, shuffle reduction |
| 13 | Caching & Persistence | `cache()`, `persist()`, `unpersist()`, Spark storage levels |
| 14 | Dynamic Resource Management | Executor memory, cores, and dynamic allocation |
| 15 | Adaptive Query Execution (AQE) | Runtime plan optimization, shuffle coalescing, skew join handling |
| 16 | Dynamic Partition Pruning | Filter pushdown, predicate pruning |
| 17 | Broadcast Variables & Accumulators | Global vars, counters, and data sharing across tasks |
| 18 | Salting & Skew Handling | Skew mitigation, adding salts, data balancing |
| 19 | Delta Lake Basics | ACID transactions, schema enforcement, Delta tables |
| 20 | Delta Optimization | Z-ordering, compaction, vacuum, `OPTIMIZE` command |


## 🔴 ADVANCED LEVEL (Days 21–30)
> *Structured Streaming, Event-time handling, and final Delta Lake project.*

| Day | Topic | Content Covered |
|:--:|:--|:--|
| 21 | Structured Streaming Introduction | Micro-batching vs continuous mode, streaming architecture |
| 22 | Stateful vs Stateless Transformations | Stateful aggregations, checkpointing |
| 23 | JSON Streaming ETL | Reading JSON streams, flattening nested structures |
| 24 | Triggers & Output Modes | Append, complete, update modes |
| 25 | ForEachBatch Logic | Custom ETL processing inside `foreachBatch` |
| 26 | Event Time & Watermarks | Handling late-arriving data, watermark thresholds |
| 27 | Windowed Streaming | Tumbling, sliding, session windows, aggregations |
| 28 | Batch + Streaming Integration | Merging static and streaming data pipelines |
| 29 | Delta Lake Final Project | Unified Delta ETL pipeline (batch + stream) |
| 30 | Wrap-up  | update README |

