


# 📘 Introduction to Spark Application, SparkSession, SparkContext & Reusing SparkSession

## What is a Spark Application?
A **Spark Application** is the complete program you run on Spark.  
It usually contains:

- **Driver program** — runs your main logic, creates `SparkSession` / `SparkContext`, and coordinates work.
- **Executors** — worker processes that execute tasks and hold data.
- **Cluster manager** — allocates resources (YARN, Kubernetes, Mesos, or Spark Standalone).

👉 Think of a Spark Application as the entire job (your Python/Scala script or notebook) submitted to Spark.

---

## What is SparkContext?
- `SparkContext` is the **low-level connection** to the Spark cluster.
- Responsibilities:
  - Talks to the cluster manager.
  - Requests executors and schedules tasks.
  - Manages distributed RDDs and execution.

Code examples:

```python
# Old style (before Spark 2.0)
from pyspark import SparkContext
sc = SparkContext(appName="MyApp")

# Modern way (via SparkSession)
sc = spark.sparkContext


## 1️⃣ What is SparkSession?
- The **entry point** to using Spark in Python.  
- Provides an interface to work with:
  - **DataFrames**
  - **SQL queries**
  - **RDDs**
- SparkSession internally manages the **SparkContext** (low-level) and SQL functionality.

---

## 2️⃣ Creating a SparkSession

```python
from pyspark.sql import SparkSession

# Create a Spark session
spark = SparkSession.builder.appName("DatapurProgram").getOrCreate()
```

✅ **Explanation**:  
- `SparkSession.builder` → used to configure your Spark application.  
- `.appName("DatapurProgram")` → assigns a name to the Spark job (visible in the Spark UI).  
- `.getOrCreate()` → creates a new session if one does not exist, otherwise reuses the existing session.  

🔎 **Result**: You now have a `spark` object to interact with Spark.  

# Spark Application, SparkSession, and SparkContext – Diagram

```
Spark Application
│
├── Driver Program
│   │
│   ├── SparkSession (High-level entry point for users)
│   │       │
│   │       └── SparkContext (Connection to cluster)
│   │               │
│   │               ├── Schedules tasks
│   │               ├── Talks to Cluster Manager
│   │               │       (YARN / Kubernetes / Standalone / Mesos)
│   │               └── Manages RDDs and executors
│   │
│   └── User Code
│           (DataFrame, SQL, RDD APIs)
│
└── Executors (Worker processes)
        │
        ├── Run tasks from SparkContext
        └── Store data (cache, shuffle, intermediate results)
```

## 3️⃣ Exploring Spark Documentation

```python
help(spark)
```

- Displays built-in documentation for the `spark` object.  
- Useful for discovering methods and properties of the SparkSession.  

```python
help(spark.createDataFrame)
```
- Shows documentation for the `createDataFrame` method.  
- This method is used to convert lists, pandas DataFrames, or RDDs into Spark DataFrames.

---

## 4️⃣ Exploring PySpark Modules

```python
import pyspark.sql.types
help(pyspark.sql)
#help(pyspark.sql.types)
```

- `pyspark.sql` → main module for Spark SQL features.  
- `pyspark.sql.types` → defines data types used in DataFrames.  
- `help()` function lets you browse available methods and documentation.

---

## 5️⃣ Defining a Schema

```python
import pyspark.sql.types

schema = pyspark.sql.types.StructType([
    pyspark.sql.types.StructField("id", pyspark.sql.types.IntegerType(), True),
    pyspark.sql.types.StructField("name", pyspark.sql.types.StringType(), True)
])

display(schema)
```

✅ **Explanation**:  
- `StructType` = a schema definition (like a table structure).  
- `StructField("id", IntegerType(), True)` → a column named `id`, type integer, nullable.  
- `StructField("name", StringType(), True)` → a column named `name`, type string, nullable.  
- `display(schema)` → shows schema details in the notebook interface.

---

## 6️⃣ Why Define a Schema?
- Ensures data types are **consistent**.  
- Prevents Spark from incorrectly inferring types.  
- Improves **performance** (Spark doesn’t need to scan the file to guess schema).  
- Useful for production ETL pipelines where data quality matters.  

---

## 🎓 Summary
- A **SparkSession** is your entry point to Spark in PySpark.  
- Use `.builder.appName().getOrCreate()` to start/reuse a session.  
- Explore objects using `help()` for quick documentation lookup.  
- Define schemas with `StructType` and `StructField` to control data structure.  
