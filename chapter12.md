---
title: 'Getting to know PySpark'
description: 'In this chapter, you''ll learn how Spark manages data and how can you read and write tables from Python.'
free_preview: true
---

## What is Spark, anyway?

```yaml
type: PureMultipleChoiceExercise
key: 89933475db
lang: python
xp: 50
skills: 2
```

Spark is a platform for cluster computing. Spark lets you spread data and computations over *clusters* with multiple *nodes* (think of each node as a separate computer). Splitting up your data makes it easier to work with very large datasets because each node only works with a small amount of data.

As each node works on its own subset of the total data, it also carries out a part of the total calculations required, so that both data processing and computation are performed *in parallel* over the nodes in the cluster. It is a fact that parallel computation can make certain types of programming tasks much faster.

However, with greater computing power comes greater complexity.

Deciding whether or not Spark is the best solution for your problem takes some experience, but you can consider questions like:

- Is my data too big to work with on a single machine?
- Can my calculations be easily parallelized?

<hr>

Are you excited to learn more about Spark?

`@hint`
Spark is great for doing parallel computations on large data sets.

`@possible_answers`
- No way!
- [Yes way!]

`@feedbacks`
- That's not a very good attitude.
- Awesome! Let's dive right in!

---

## Using Spark in Python

```yaml
type: PureMultipleChoiceExercise
key: 1db89644d0
lang: python
xp: 50
skills: 2
```

The first step in using Spark is connecting to a cluster.

In practice, the cluster will be hosted on a remote machine that's connected to all other nodes. There will be one computer, called the *master* that manages splitting up the data and the computations. The master is connected to the rest of the computers in the cluster, which are called *slaves*. The master sends the slaves data and calculations to run, and they send their results back to the master.

When you're just getting started with Spark it's simpler to just run a cluster locally. Thus, for this course, instead of connecting to another computer, all computations will be run on DataCamp's servers in a simulated cluster.

Creating the connection is as simple as creating an instance of the `SparkContext` class. The class constructor takes a few optional arguments that allow you to specify the attributes of the cluster you're connecting to.

An object holding all these attributes can be created with the `SparkConf()` constructor. Take a look at the [documentation](http://spark.apache.org/docs/2.1.0/api/python/pyspark.html) for all the details!

For the rest of this course you'll have a `SparkContext` called `sc` already available in your workspace.

<hr>

How do you connect to a Spark cluster from PySpark?

`@hint`
You need to use a class from the `pyspark` module.

`@possible_answers`
- [Create an instance of the `SparkContext` class.]
- Import the `pyspark` library.
- Plug your computer into the cluster.

`@feedbacks`
- Great job! I knew you were paying attention.
- Yes, you do have to do that, but typing `import pyspark` won't connect you to a cluster.
- What kind of plug does a Spark cluster have?

---

## Examining The SparkContext

```yaml
type: SingleProcessExercise
key: 4d7fb3765a
lang: python
xp: 100
skills: 2
```

In this exercise you'll get familiar with the `SparkContext`.

You'll probably notice that code takes longer to run than you might expect. This is because Spark is some serious software. It takes more time to start up than you might be used to. You may also find that running simpler computations might take longer than expected. That's because all the optimizations that Spark has under its hood are designed for complicated operations with big data sets. That means that for simple or small problems Spark may actually perform worse than some other solutions!

`@instructions`
Get to know the `SparkContext`.

- Call `print()` on `sc` to verify there's a `SparkContext` in your environment.
- `print()` `sc.version` to see what version of Spark is running on your cluster.

`@hint`
Remember, we've already set up a `SparkContext` for you!

`@pre_exercise_code`
```{python}
_init_spark = '/home/repl/.init-spark.py'
with open(_init_spark) as f:
    code = compile(f.read(), _init_spark, 'exec')
    exec(code)
```

`@sample_code`
```{python}
# Verify SparkContext
print(____)

# Print Spark version
print(____)
```

`@solution`
```{python}
# Verify SparkContext
print(sc)

# Print Spark version
print(sc.version)
```

`@sct`
```{python}
Ex().has_equal_ast(code="print(sc)", exact=False, incorrect_msg="Did you examine `sc`?")
Ex().has_equal_ast(code="print(sc.version)", exact=False, incorrect_msg="Did you print `sc.version`?")
success_msg("Awesome! You're up and running with Spark.")
```

---

## Using DataFrames

```yaml
type: PureMultipleChoiceExercise
key: c319522de9
lang: python
xp: 50
skills: 2
```

Spark's core data structure is the Resilient Distributed Dataset (RDD). This is a low level object that lets Spark work its magic by splitting data across multiple nodes in the cluster. However, RDDs are hard to work with directly, so in this course you'll be using the Spark DataFrame abstraction built on top of RDDs.

The Spark DataFrame was designed to behave a lot like a SQL table (a table with variables in the columns and observations in the rows). Not only are they easier to understand, DataFrames are also more optimized for complicated operations than RDDs.

When you start modifying and combining columns and rows of data, there are many ways to arrive at the same result, but some often take much longer than others. When using RDDs, it's up to the data scientist to figure out the right way to optimize the query, but the DataFrame implementation has much of this optimization built in!

To start working with Spark DataFrames, you first have to create a `SparkSession` object from your `SparkContext`. You can think of the `SparkContext` as your connection to the cluster and the `SparkSession` as your interface with that connection.

Remember, for the rest of this course you'll have a `SparkSession` called `spark` available in your workspace!

<hr>

Which of the following is an advantage of Spark DataFrames over RDDs?

`@hint`
Remember, DataFrames are more optimized for complicated operations.

`@possible_answers`
- [Operations using DataFrames are automatically optimized.]
- They are smaller.
- They can perform more kinds of operations.
- They can hold more kinds of data.

`@feedbacks`
- Exactly! This is another way DataFrames are like SQL tables.
- They are more complicated than RDDs, so it doesn't seem like they'd be any smaller.
- DataFrames are implemented using RDDs, so anything you can do with a DataFrame, you can also do with an RDD.
- DataFrames are implemented using RDDs, so anything you can do with a DataFrame, you can also do with an RDD.

---

## Creating a SparkSession

```yaml
type: SingleProcessExercise
key: 4ecd1c0668
lang: python
xp: 100
skills: 2
```

We've already created a `SparkSession` for you called `spark`, but what if you're not sure there already is one? Creating multiple `SparkSession`s and `SparkContext`s can cause issues, so it's best practice to use the `SparkSession.builder.getOrCreate()` method. This returns an existing `SparkSession` if there's already one in the environment, or creates a new one if necessary!

`@instructions`
- Import `SparkSession` from `pyspark.sql`.
- Make a new `SparkSession` called `my_spark` using `SparkSession.builder.getOrCreate()`.
- Print `my_spark` to the console to verify it's a `SparkSession`.

`@hint`
Try using `.getOrCreate()` to get a new `SparkSession`.

`@pre_exercise_code`
```{python}
_init_spark = '/home/repl/.init-spark.py'
with open(_init_spark) as f:
    code = compile(f.read(), _init_spark, 'exec')
    exec(code)
```

`@sample_code`
```{python}
# Import SparkSession from pyspark.sql
from ____ import ____

# Create my_spark
my_spark = ____

# Print my_spark
print(____)
```

`@solution`
```{python}
# Import SparkSession from pyspark.sql
from pyspark.sql import SparkSession

# Create my_spark
my_spark = SparkSession.builder.getOrCreate()

# Print my_spark
print(my_spark)
```

`@sct`
```{python}
Ex().has_equal_ast(code="from pyspark.sql import SparkSession", exact=False, incorrect_msg="Did you correctly import `SparkSession`?")
Ex().has_equal_ast(code="my_spark = SparkSession.builder.getOrCreate()", exact=False, incorrect_msg="Did you call `SparkSession.builder.getOrCreate()`?")
Ex().has_equal_ast(code="print(my_spark)", exact=False, incorrect_msg="Did you print `my_spark`?")
success_msg("Great work! You did that like a PySpark Pro!")
```

---

## Viewing tables

```yaml
type: SingleProcessExercise
key: 09014a8e63
lang: python
xp: 100
skills: 2
```

Once you've created a `SparkSession`, you can start poking around to see what data is in your cluster!

Your `SparkSession` has an attribute called `catalog` which lists all the data inside the cluster. This attribute has a few methods for extracting different pieces of information.

One of the most useful is the `.listTables()` method, which returns the names of all the tables in your cluster as a list.

`@instructions`
- See what tables are in your cluster by calling `spark.catalog.listTables()` and printing the result!

`@hint`
Try calling `.listTables()` on the `catalog` attribute!

`@pre_exercise_code`
```{python}
_init_spark = '/home/repl/.init-spark.py'
with open(_init_spark) as f:
    code = compile(f.read(), _init_spark, 'exec')
    exec(code)

temp = spark.read.csv("/usr/local/share/datasets/flights.csv", header = True)
temp.createOrReplaceTempView("flights")
```

`@sample_code`
```{python}
# Print the tables in the catalog
print(spark.____.____())
```

`@solution`
```{python}
# Print the tables in the catalog
print(spark.catalog.listTables())
```

`@sct`
```{python}
Ex().check_function('spark.catalog.listTables', 0, signature=False, missing_msg='Did you show the tables using `listTables()` on `catalog` correctly?')
Ex().has_equal_ast(code='print(spark.catalog.listTables())', exact=False, incorrect_msg='Did you print the tables?')

success_msg("Fantastic! What kind of data do you think is in that table?")
```

---

## Are you query-ious?

```yaml
type: SingleProcessExercise
key: 1d20941df8
lang: python
xp: 100
skills: 2
```

One of the advantages of the DataFrame interface is that you can run SQL queries on the tables in your Spark cluster. If you don't have any experience with SQL, don't worry (you can take our [Introduction to SQL](https://www.datacamp.com/courses/intro-to-sql-for-data-science) course!), we'll provide you with queries!

As you saw in the last exercise, one of the tables in your cluster is the `flights` table. This table contains a row for every flight that left Portland International Airport (PDX) or Seattle-Tacoma International Airport (SEA) in 2014 and 2015.

Running a query on this table is as easy as using the `.sql()` method on your `SparkSession`. This method takes a string containing the query and returns a DataFrame with the results!

If you look closely, you'll notice that the table `flights` is only mentioned in the query, not as an argument to any of the methods. This is because there isn't a local object in your environment that holds that data, so it wouldn't make sense to pass the table as an argument.

Remember, we've already created a `SparkSession` called `spark` in your workspace.

`@instructions`
- Use the `.sql()` method to get the first 10 rows of the `flights` table and save the result to `flights10`. The variable `query` contains the appropriate SQL query.
- Use the DataFrame method `.show()` to print `flights10`.

`@hint`
Remember you can run a query by doing `spark.sql(a_query)`!

`@pre_exercise_code`
```{python}
_init_spark = '/home/repl/.init-spark.py'
with open(_init_spark) as f:
    code = compile(f.read(), _init_spark, 'exec')
    exec(code)

temp = spark.read.csv("/usr/local/share/datasets/flights.csv", header = True)
temp.createOrReplaceTempView("flights")
```

`@sample_code`
```{python}
# Don't change this query
query = "FROM flights SELECT * LIMIT 10"

# Get the first 10 rows of flights
flights10 = ____

# Show the results
flights10.____
```

`@solution`
```{python}
# Don't change this query
query = "FROM flights SELECT * LIMIT 10"

# Get the first 10 rows of flights
flights10 = spark.sql(query)

# Show the results
flights10.show()
```

`@sct`
```{python}
Ex().has_equal_ast(code='query = "FROM flights SELECT * LIMIT 10"', exact=False, incorrect_msg='Do not change the SQL query!')
Ex().has_equal_ast(code='flights10 = spark.sql(query)', exact=False, incorrect_msg='Did you define `flights10` correctly?')
Ex().check_function('flights10.show', 0, signature=False, missing_msg='Did you call `.show()` on `flights10` correctly?')

success_msg("Awesome work! You've got queries down!")
```

---

## Pandafy a Spark DataFrame

```yaml
type: SingleProcessExercise
key: e1c5e047e0
lang: python
xp: 100
skills: 2
```

Suppose you've run a query on your huge dataset and aggregated it down to something a little more manageable.

Sometimes it makes sense to then take that table and work with it locally using a tool like `pandas`. Spark DataFrames make that easy with the `.toPandas()` method. Calling this method on a Spark DataFrame returns the corresponding `pandas` DataFrame. It's as simple as that!

This time the query counts the number of flights to each airport from SEA and PDX.

Remember, there's already a `SparkSession` called `spark` in your workspace!

`@instructions`
- Run the query using the `.sql()` method. Save the result in `flight_counts`.
- Use the `.toPandas()` method on `flight_counts` to create a `pandas` DataFrame called `pd_counts`.
- Print the `.head()` of `pd_counts` to the console.

`@hint`
Remember, you can make a `DataFrame` using `.toPandas()`.

`@pre_exercise_code`
```{python}
_init_spark = '/home/repl/.init-spark.py'
with open(_init_spark) as f:
    code = compile(f.read(), _init_spark, 'exec')
    exec(code)

import pandas as pd
temp = spark.read.csv("/usr/local/share/datasets/flights.csv", header = True)
temp.createOrReplaceTempView("flights")
```

`@sample_code`
```{python}
# Don't change this query
query = "SELECT origin, dest, COUNT(*) as N FROM flights GROUP BY origin, dest"

# Run the query
flight_counts = ____

# Convert the results to a pandas DataFrame
pd_counts = _____

# Print the head of pd_counts
print(____)
```

`@solution`
```{python}
# Don't change this query
query = "SELECT origin, dest, COUNT(*) as N FROM flights GROUP BY origin, dest"

# Run the query
flight_counts = spark.sql(query)

# Convert the results to a pandas DataFrame
pd_counts = flight_counts.toPandas()

# Print the head of pd_counts
print(pd_counts.head())

```

`@sct`
```{python}
Ex().has_equal_ast(code='query = "SELECT origin, dest, COUNT(*) as N FROM flights GROUP BY origin, dest"', exact=False, incorrect_msg='Do not change the SQL query!')
Ex().has_equal_ast(code='flight_counts = spark.sql(query)', exact=False, incorrect_msg='Did you define `flight_counts` correctly?')
Ex().check_function('flight_counts.toPandas', 0, signature=False, missing_msg='Did you call `toPandas()` on `flight_counts`?')
Ex().has_equal_ast(code='print(pd_counts.head())', exact=False, incorrect_msg='Did you print a preview of `pd_counts` correctly?')

success_msg("Great job! You did it!")
```

---

## Put some Spark in your data

```yaml
type: SingleProcessExercise
key: 4ec36034a9
lang: python
xp: 100
skills: 2
```

In the last exercise, you saw how to move data from Spark to `pandas`. However, maybe you want to go the other direction, and put a `pandas` DataFrame into a Spark cluster! The `SparkSession` class has a method for this as well.

The `.createDataFrame()` method takes a `pandas` DataFrame and returns a Spark DataFrame.

The output of this method is stored locally, not in the `SparkSession` catalog. This means that you can use all the Spark DataFrame methods on it, but you can't access the data in other contexts.

For example, a SQL query (using the `.sql()` method) that references your DataFrame will throw an error. To access the data in this way, you have to save it as a *temporary table*.

You can do this using the `.createTempView()` Spark DataFrame method, which takes  as its only argument the name of the temporary table you'd like to register. This method registers the DataFrame as a table in the catalog, but as this table is temporary, it can only be accessed from the specific `SparkSession` used to create the Spark DataFrame.

There is also the method `.createOrReplaceTempView()`. This safely creates a new temporary table if nothing was there before, or updates an existing table if one was already defined. You'll use this method to avoid running into problems with duplicate tables.

Check out the diagram to see all the different ways your Spark data structures interact with each other.

![](https://s3.amazonaws.com/assets.datacamp.com/production/course_4452/datasets/spark_figure.png)

There's already a `SparkSession` called `spark` in your workspace, `numpy` has been imported as `np`, and `pandas` as `pd`.

`@instructions`
- The code to create a `pandas` DataFrame of random numbers has already been provided and saved under `pd_temp`.
- Create a Spark DataFrame called `spark_temp` by calling the `.createDataFrame()` method with `pd_temp` as the argument.
- Examine the list of tables in your Spark cluster and verify that the new DataFrame is *not* present. Remember you can use `spark.catalog.listTables()` to do so.
- Register `spark_temp` as a temporary table named `"temp"` using the `.createOrReplaceTempView()` method. Rememeber that the table name is set including it as the only argument!
- Examine the list of tables again!

`@hint`
Remember, you can use `.createDataFrame()` to create a Spark DataFrame.

`@pre_exercise_code`
```{python}
_init_spark = '/home/repl/.init-spark.py'
with open(_init_spark) as f:
    code = compile(f.read(), _init_spark, 'exec')
    exec(code)

import pandas as pd
import numpy as np
```

`@sample_code`
```{python}
# Create pd_temp
pd_temp = pd.DataFrame(np.random.random(10))

# Create spark_temp from pd_temp
spark_temp = _____

# Examine the tables in the catalog
print(____)

# Add spark_temp to the catalog
spark_temp.____

# Examine the tables in the catalog again
print(____)
```

`@solution`
```{python}
# Create pd_temp
pd_temp = pd.DataFrame(np.random.random(10))

# Create spark_temp from pd_temp
spark_temp = spark.createDataFrame(pd_temp)

# Examine the tables in the catalog
print(spark.catalog.listTables())

# Add spark_temp to the catalog
spark_temp.createOrReplaceTempView("temp")

# Examine the tables in the catalog again
print(spark.catalog.listTables())
```

`@sct`
```{python}
Ex().has_equal_ast(code='pd_temp = pd.DataFrame(np.random.random(10))', exact=False, incorrect_msg='Did you define `pd_temp` correctly?')
Ex().has_equal_ast(code='spark_temp = spark.createDataFrame(pd_temp)', exact=False, incorrect_msg='Did you define `spark_temp` correctly?')
Ex().has_equal_ast(code='print(spark.catalog.listTables())', exact=False, incorrect_msg='Did you call `print(spark.catalog.listTables())`?')
Ex().has_equal_ast(code='spark_temp.createOrReplaceTempView("temp")', exact=False, incorrect_msg='Did you call `.createOrReplaceTempView()` on `spark_temp` correctly?')
Ex().has_equal_ast(code='print(spark.catalog.listTables())', exact=False, incorrect_msg='Did you print the tables in the catalog correctly again?')

success_msg("Awesome! Now you can get your data in and out of Spark.")
```

---

## Dropping the middle man

```yaml
type: SingleProcessExercise
key: 185e9178ea
lang: python
xp: 100
skills: 2
```

Now you know how to put data into Spark via `pandas`, but you're probably wondering why deal with `pandas` at all? Wouldn't it be easier to just read a text file straight into Spark? Of course it would!

Luckily, your `SparkSession` has a `.read` attribute which has several methods for reading different data sources into Spark DataFrames. Using these you can create a DataFrame from a .csv file just like with regular `pandas` DataFrames!

The variable `file_path` is a string with the path to the file `airports.csv`. This file contains information about different airports all over the world.

A `SparkSession` named `spark` is available in your workspace.

`@instructions`
- Use the `.read.csv()` method to create a Spark DataFrame called `airports`
    - The first argument is `file_path`
    - Pass the argument `header=True` so that Spark knows to take the column names from the first line of the file.
- Print out this DataFrame by calling `.show()`.

`@hint`
Remember to pass both arguments to `.read.csv()`!

`@pre_exercise_code`
```{python}
_init_spark = '/home/repl/.init-spark.py'
with open(_init_spark) as f:
    code = compile(f.read(), _init_spark, 'exec')
    exec(code)
```

`@sample_code`
```{python}
# Don't change this file path
file_path = "/usr/local/share/datasets/airports.csv"

# Read in the airports data
airports = _____

# Show the data

```

`@solution`
```{python}
# Don't change this file path
file_path = "/usr/local/share/datasets/airports.csv"

# Read in the airports data
airports = spark.read.csv(file_path, header=True)

# Show the data
airports.show()
```

`@sct`
```{python}
Ex().has_equal_ast(code="file_path = '/usr/local/share/datasets/airports.csv'", exact=False, incorrect_msg="Don't change the file path.")
Ex().has_equal_ast(code="airports = spark.read.csv(file_path, header=True)", exact=False, incorrect_msg="Check your call of `spark.read.csv()`.")
Ex().has_equal_ast(code="airports.show()", exact=False, incorrect_msg="Make sure to show your result.")

success_msg("Awesome job! You've got the basics of Spark under your belt!")

```
