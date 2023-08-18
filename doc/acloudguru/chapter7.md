# Chapter 7. Databases

<!-- TOC -->

- [Chapter 7. Databases](#chapter-7-databases)
  - [RDS Overview](#rds-overview)
    - [OLTP vs. OLAP](#oltp-vs-olap)
    - [What is Multi-AZ?](#what-is-multi-az)
    - [Unplanned Failure or Maintenance](#unplanned-failure-or-maintenance)
  - [Increasing Read Performance with Read Replicas](#increasing-read-performance-with-read-replicas)
    - [Exam Tips](#exam-tips)
  - [What is Amazon Aurora?](#what-is-amazon-aurora)
    - [Basics](#basics)
    - [Aurora Replicas](#aurora-replicas)
    - [Aurora Serverless](#aurora-serverless)
  - [DynamoDB Overview](#dynamodb-overview)
    - [High-Level DynamoDB](#high-level-dynamodb)
    - [DynamoDB Accelerator DAX](#dynamodb-accelerator-dax)
    - [On-Demand Capacity](#on-demand-capacity)
    - [Security](#security)
  - [When Do We Use DynamoDB Transactions?](#when-do-we-use-dynamodb-transactions)
  - [Saving Your Data with DynamoDB Backups](#saving-your-data-with-dynamodb-backups)
  - [Taking Your Data Global with DynamoDB Streams and Global Tables](#taking-your-data-global-with-dynamodb-streams-and-global-tables)
  - [Operating MongoDB-Compatible Databases in Amazon DocumentDB](#operating-mongodb-compatible-databases-in-amazon-documentdb)
  - [Running Apache Cassandra Workloads with Amazon Keyspaces](#running-apache-cassandra-workloads-with-amazon-keyspaces)
  - [Implementing Graph Databases Using Amazon Neptune](#implementing-graph-databases-using-amazon-neptune)
  - [Leveraging Amazon Quantum Ledger Database QLDB for Ledger Databases](#leveraging-amazon-quantum-ledger-database-qldb-for-ledger-databases)
  - [Analyzing Time-Series Data with with Amazon Timestream](#analyzing-time-series-data-with-with-amazon-timestream)
  - [Exam Tips](#exam-tips)
  - [Lab 7. Set Up a WordPress Site Using EC2 and RDS](#lab-7-set-up-a-wordpress-site-using-ec2-and-rds)
    - [Introduction](#introduction)

<!-- /TOC -->

---
## RDS Overview

Relational Database System (RDS) is generally used for Online Transaction Processing (OLTP) workloads, but not suitable for analyzing large amounts of data or Online Analytical Processing (OLAP). Use a data warehouse like **RedShift**, which is optimized for OLAP.

### OLTP vs. OLAP

* Online Transaction Processing (OLTP):
  - **Processes data from transactions in real time**, e.g. customer orders, banking transactions, payments, and booking systems.
  - All about data processing and completing large numbers of small transactions in real time.

* Online Analytical Processing (OLAP):
  - **Processes complex queries to analyze historical data**, e.g. analyzing net profit figures from the past 3 years and sales forecasting.
  - All about data analysis using large amounts of data, as well as complex queries that take a long time to complete.

### What is Multi-AZ?

With Multi-AZ, RDS creates an exact copy of your production database in another AZ in real-time. Which RDS types can be configured as Multi-AZ?
* SQL Server
* MySQL
* MariaDB
* Oracle
* PostgreSQL
* Amazon Aurora (always Multi-AZ)

**Multi-AZ is for disaster recovery**, not for improving performance, so you cannot connect to the standby when the primary database is active.

### Unplanned Failure or Maintenance

If there is an unplanned failure, AWS will automatically detects the unhealthy RDS primary database. Keeping the same RDS endpoint, AWS will route new transactions to the RDS secondary database (DNS fail over).

The standby RDS will be automatically promoted as the primary database so database operations can resume quickly without administrative intervention.

---
## Increasing Read Performance with Read Replicas

A read replica is a read-only copy of your primary database. It is useful for read-heavy workloads and takes the load-off your primary database.

> Exam Tip: Read replicas complement Multi-AZ deployments. The **main purpose of read replicas is scalability**, whereas the main purpose of Multi-AZ deployments is availability. However, you may **use a read replica for disaster recovery** of the source DB instance either in the same AWS region or in another region.

Each read replica has its own DNS endpoint. **Read replicas can be promoted to be their own databases**, however this breaks the replication to the primary database.

Key Facts:

* Scaling Read Performance: Primarily used for scaling, not for disaster recovery.

* Requires Automatic Backup: Automatic backups must be enabled in order to deploy a read replica.

* Multiple Read Replicas are Supported: MySQL, MariaDB, PostgreSQL, Oracle, and SQL Server allow you to add up to 5 read replicas to each DB instance.

### Exam Tips

* Multi-AZ
  - An exact copy of your production database in another AZ
  - Used for disaster recovery
  - In the event of a failure, RDS will automatically fail over to the standby instance.

* Read Replica
  - A read-only copy of your primary database in the same AZ, cross-AZ, or cross-region.
  - Used to increase or scale read performance.
  - Great for read-heavy workloads and takes the load off your primary database for read-only workloads, e.g. business intelligence reporting jobs.

---
## What is Amazon Aurora?

AWS Aurora is a MySQL and PostgreSQL-compatible RDS that was developed by Amazon. Aurora provides up to 5x and 3x **better performance than MySQL and PostgreSQL databases** respectively, at a much lower price point.

### Basics

* Starts with 10 GB data storage, and scales in 10 GB increments to 128 TB (storage auto scaling).

* Compute resources can scale up to 96 vCPUs and 768 GB of memory.

* Two copies of your data are contained in each AZ, with a minimum of three AZs, i.e. minimum of 6 copies of your data in HA.

* Loss of up to 2 copies of data without affecting database write availability, and up to 3 copies without affecting read availability.

* Aurora is also self-healing, where **data blocks and disks are continuously scanned for errors** and repaired automatically.

### Aurora Replicas

Three types of read replicas:

* Aurora Replicas - You can have up to 15 read replicas (in-region) with automated failover.

* MySQL Replicas - You can have up to 5 read replicas (cross-region), but without automated failover.

* PostgreSQL Replicas - You can have up to 5 read replicas (cross-region), but without automated failover.

### Aurora Serverless

AWS Aurora Serverless is an on-demand, auto-scaling configuration for the MySQL-compatible and PostgreSQL-compatible editions of Aurora. An **Aurora Serverless DB cluster automatically starts up, shuts down, and scales capacity up or down** based on your application's needs.

Aurora Serverless provides a relatively simple, cost-effective option for infrequent, intermittent, or unpredictable workloads.

---
## DynamoDB Overview

AWS DynamoDB is a proprietary NoSQL database service for all applications that need consistent, single-digit millisecond latency at any scale. It is a fully managed database and supports both document and key-value data models.

Its flexible data model and reliable performance make it a great fit for mobile, web, gaming, ad-tech, IoT, etc.

### High-Level DynamoDB

* Data stored on SSD storage.

* Spread across three geographically distinct data centres.

**Read Consistency**

* Eventually consistent reads (default) - copies of data usually reached within 1 second for best read performance.

* Strongly consistent reads - returns a result that reflects all writes that received a successful response prior to a read.

### DynamoDB Accelerator (DAX)

* Fully managed, HA, in-memory cache

* 10x performance improvement

* Reduces request time from milliseconds to microseconds

* Compatible with DynamoDB API calls

### On-Demand Capacity

* Pay-per-request pricing

* No minimum capacity

* Pay more per request than with provisioned capacity

* Use for new product launches

### Security

* Encryption at rest using KMS

* Site-to-site VPN

* Direct Connect (DX)

* IAM policies and roles

* Fine-grained access

* CloudWatch and CloudTrail

* VPC endpoints to directly communicate with DynamoDB within a Amazon network layer

---
## When Do We Use DynamoDB Transactions?

---
## Saving Your Data with DynamoDB Backups

---
## Taking Your Data Global with DynamoDB Streams and Global Tables

---
## Operating MongoDB-Compatible Databases in Amazon DocumentDB

---
## Running Apache Cassandra Workloads with Amazon Keyspaces

---
## Implementing Graph Databases Using Amazon Neptune

---
## Leveraging Amazon Quantum Ledger Database (QLDB) for Ledger Databases

---
## Analyzing Time-Series Data with with Amazon Timestream

---
## Exam Tips

---
## Lab 7. Set Up a WordPress Site Using EC2 and RDS

<details>
<summary>Click here to start Lab 7.</summary>

### Introduction

</details>