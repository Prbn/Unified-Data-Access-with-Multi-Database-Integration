# Unified-Data-Access-with-Multi-Database-Integration

This project demonstrates the implementation and integration of various relational and non-relational databases, including Apache Spark, MongoDB, HBase, Cassandra, Redis, Kafka, Neo4j, and Elasticsearch. The goal is to provide seamless data access and efficient management across diverse data sources using SQL-based querying via Apache Drill.

### Group:
- Indra Ariunbold
- Prabin Shrestha
- Hendi Kushta

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Setup](#setup)
  - [Docker Compose Configuration](#docker-compose-configuration)
  - [Connection via Native Client](#connection-via-native-client)
  - [CRUD Operations](#crud-operations)
  - [Connection to Spark](#connection-to-spark)
  - [Connection to Drill](#connection-to-apache-drill)
- [References](#references)

## Overview

Apache HBase is a free, open-source, non-relational database designed for large-scale data storage and real-time access. It is particularly well-suited for scenarios requiring high throughput and low-latency access to massive datasets. HBase is part of the Hadoop ecosystem, providing seamless integration with Hadoop’s distributed file system (HDFS).

### Key Benefits:

- **Scalability**: Capable of scaling across thousands of servers to manage petabytes of data.
- **Speed**: Supports real-time access with low-latency for large datasets.
- **Fault-tolerance**: Resilient to server failures with automatic redistribution of data.

### Use Cases

- **FINRA**: Real-time access to trillions of financial records stored on Amazon S3.
- **Monster**: Stores clickstream and advertising data for detailed campaign analytics.

## Architecture

HBase’s architecture is designed to manage and retrieve massive amounts of distributed data efficiently. Key components include:

- **Data Model**: Flexible schema organized into tables with rows and column families.
- **Region**: Subdivides data storage to distribute the workload across multiple servers.
- **HMaster**: Oversees the assignment of regions to region servers.
- **Region Server**: Handles read/write requests and stores actual data on disk.
- **ZooKeeper**: Coordinates and maintains system metadata.

For a more detailed explanation of these components, please refer to the [Apache HBase Documentation](https://hbase.apache.org/book.html).

## Setup

### Docker Compose Configuration

To run the Apache HBase environment, we use Docker Compose to manage the necessary services. The following command will start the HBase shell in a Docker container:

```bash
docker-compose exec hbase hbase shell
```

### Connection via Native Client

After starting the Docker environment, you can interact with HBase through its shell for performing CRUD operations.

### CRUD Operations

1. **Create Table**  
   Create a simple `student` table with `info` as the column family:
   ```bash
   create 'student', 'info'
   ```

2. **Insert Data**  
   Insert data for three students with attributes such as `id`, `name`, `age`, `gender`, and `grade`:
   ```bash
   put 'student', '1', 'info:name', 'John'
   put 'student', '1', 'info:age', '20'
   put 'student', '1', 'info:gender', 'Male'
   put 'student', '1', 'info:grade', 'A'
   ```

3. **Read Data**  
   Read data for a specific student:
   ```bash
   get 'student', '1'
   ```

4. **Update Data**  
   Update the age of a student:
   ```bash
   put 'student', '2', 'info:age', '23'
   ```

5. **Delete Data**  
   Delete a student record:
   ```bash
   delete 'student', '3'
   ```

You can also monitor your tables using the HBase Master Web UI at [http://localhost:16010/master-status](http://localhost:16010/master-status).

### Connection to Spark

Although we attempted to connect HBase with Spark, we encountered challenges in successfully establishing the connection. Some resources we explored:

- **[Spark HBase Connector Notes](https://github.com/LucaCanali/Miscellaneous/blob/master/Spark_Notes/Spark_HBase_Connector.md)**
- **[StackOverflow Discussion](https://stackoverflow.com/questions/38470114/how-to-connect-hbase-and-spark-using-python/38575095)**

We recommend these guides for further exploration.

### Connection to Apache Drill

To query HBase using Apache Drill, we configured a `drill-storage.json` file with the necessary HBase connection information. Once configured, you can query the data stored in HBase using SQL-like syntax.

An example query to retrieve student data:

```sql
SELECT 
  CAST(ROW_KEY AS INT) AS id,
  CAST(CAST(CAST(info['age'] AS BINARY) AS VARCHAR) AS INT) AS age,
  CAST(CAST(CAST(info['gender'] AS BINARY) AS VARCHAR) AS VARCHAR) AS gender,
  CAST(CAST(CAST(info['grade'] AS BINARY) AS VARCHAR) AS VARCHAR) AS grade,
  CAST(CAST(CAST(info['name'] AS BINARY) AS VARCHAR) AS VARCHAR) AS name
FROM hbase.`student`;
```

## References

1. **Apache HBase TM Reference Guide**: [https://hbase.apache.org/book.html](https://hbase.apache.org/book.html)
2. **What is HBase? - Apache HBase Explained on AWS**: [https://aws.amazon.com/what-is/apache-hbase/](https://aws.amazon.com/what-is/apache-hbase/)
3. **Overview of HBase Architecture and its Components**: [https://www.projectpro.io/article/overview-of-hbase-architecture-and-its-components/295](https://www.projectpro.io/article/overview-of-hbase-architecture-and-its-components/295)
