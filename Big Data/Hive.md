# Hive
The Apache Hive data warehouse software facilitates reading, writing, and managing large datasets residing in distributed storage using SQL. Structure can be projected onto data already in storage. A command line tool and JDBC driver are provided to connect users to Hive.

	• Provides a SQL interface, known as HiveQL (HQL) for easy querying in Hadoop
	• Has Data Definition Language (DDL) and Data Manipulation Language (DML)
	• Supports all common primitive data formats

## Main Access Methods

__Hadoop Edge Node__

Beeline should be used when interacting with Hive via the Edge Node command line.

If you do not start "beeline" you will be trying to access Hive Metastore and HDFS using Hive CLI Apache Thrift based client as opposed to the beeline JDBC client that is preferred method with HiveServer2.

## Queries

- To Show Tables ```hive> show tables;```

- Drop Table ``` hive> drop table table_name;```

## Script Execution

```hive -f script.sql ```
