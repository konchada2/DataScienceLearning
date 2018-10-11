# PySpark

Spark is a platform for cluster computing. Spark lets you spread data and computations over clusters with multiple nodes (think of each node as a separate computer). Splitting up your data makes it easier to work with very large datasets because each node only works with a small amount of data.

As each node works on its own subset of the total data, it also carries out a part of the total calculations required, so that both data processing and computation are performed in parallel over the nodes in the cluster. It is a fact that parallel computation can make certain types of programming tasks much faster.

In practice, the cluster will be hosted on a remote machine that's connected to all other nodes. There will be one computer, called the master that manages splitting up the data and the computations. The master is connected to the rest of the computers in the cluster, which are called slaves. The master sends the slaves data and calculations to run, and they send their results back to the master.

An object holding all these attributes can be created with the __SparkConf()__ constructor

To connect to a Spark cluster from PySpark we need to create an instance of the __SparkContext__ class.

__Spark will perform worse for smaller problems compared to big data sets.__

- Spark's core data structure is the Resilient Distributed Dataset (RDD). This is a low level object that lets Spark work its magic by splitting data across multiple nodes in the cluster. 
- DataFrames are also more optimized for complicated operations than RDDs. 

What is the advantage of Spark DataFrames over RDDs?
- Operations using DataFrames are automatically optimized.

To start working with Spark DataFrames, you first have to create a __SparkSession__ object from your __SparkContext__. You can think of the SparkContext as your connection to the cluster and the SparkSession as your interface with that connection.

Creating multiple SparkSessions and SparkContexts can cause issues, so it's best practice to use the __SparkSession.builder.getOrCreate() method__. This returns an existing SparkSession if there's already one in the environment, or creates a new one if necessary!

```
# Import SparkSession from pyspark.sql
from pyspark.sql import SparkSession

# Create my_spark
my_spark = SparkSession.builder.getOrCreate()

# Print my_spark
print(my_spark)
```
Output
```<pyspark.sql.session.SparkSession object at 0x7fe3884fe9e8>```

Your SparkSession has an attribute called __catalog__ which lists all the data inside the cluster. This attribute has a few methods for extracting different pieces of information.

One of the most useful is the __.listTables()__ method, which returns the names of all the tables in your cluster as a list.

```
# Print the tables in the catalog
print(spark.catalog.listTables())
```
Output ```[Table(name='flights', database=None, description=None, tableType='TEMPORARY', isTemporary=True)]```

```
query = "FROM flights SELECT * LIMIT 10"

# Get the first 10 rows of flights
flights10 = spark.sql(query)

# Show the results
flights10.show()
```
