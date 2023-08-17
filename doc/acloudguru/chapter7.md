# Chapter 8. Databases

<!-- TOC -->

- [Chapter 8. Databases](#chapter-8-databases)
  - [RDS Overview](#rds-overview)
    - [OLTP vs. OLAP](#oltp-vs-olap)
    - [What is Multi-AZ?](#what-is-multi-az)
    - [Unplanned Failure or Maintenance](#unplanned-failure-or-maintenance)
  - [Increasing Read Performance with Read Replicas](#increasing-read-performance-with-read-replicas)
    - [Exam Tips](#exam-tips)
  - [What is Amazon Aurora?](#what-is-amazon-aurora)
  - [DynamoDB Overview](#dynamodb-overview)
  - [When Do We Use DynamoDB Transactions?](#when-do-we-use-dynamodb-transactions)
  - [Saving Your Data with DynamoDB Backups](#saving-your-data-with-dynamodb-backups)
  - [Taking Your Data Global with DynamoDB Streams and Global Tables](#taking-your-data-global-with-dynamodb-streams-and-global-tables)
  - [Exam Tips](#exam-tips)
  - [Lab 8. Set Up a WordPress Site Using EC2 and RDS](#lab-8-set-up-a-wordpress-site-using-ec2-and-rds)
    - [Introduction](#introduction)

<!-- /TOC -->

---
## RDS Overview

Relational Database System (RDS) is not suitable for analyzing large amounts of data. Use a data warehouse like **RedShift**, which is optimized for OLAP.

### OLTP vs. OLAP

* Online Transaction Processing (OLTP):
  - Processes data from transactions in real time, e.g. customer orders, banking transactions, payments, and booking systems.
  - All about data processing and completing large numbers of small transactions in real time.

* Online Analytical Processing (OLAP): 
  - Processes complex queries to analyze historical data, e.g. analyzing net profit figures from the past 3 years and sales forecasting.
  - All about data analysis using large amounts of data, as well as complex queries that take a long time to complete.

### What is Multi-AZ?

With Multi-AZ, RDS creates an exact copy of your production database in another AZ in real-time. Which RDS types can be configured as Multi-AZ?
* SQL Server
* MySQL
* MariaDB
* Oracle
* PostgreSQL
* Amazon Aurora (always Multi-AZ)

Multi-AZ is for disaster recovery, not for improving performance, so you cannot connect to the standby when the primary database is active.

### Unplanned Failure or Maintenance

If there is an unplanned failure, AWS will automatically detects the unhealthy RDS primary database. Keeping the same RDS endpoint, AWS will route new transactions to the RDS secondary database (DNS fail over).

The standby RDS will be automatically promoted as the primary database so database operations can resume quickly without administrative intervention.

---
## Increasing Read Performance with Read Replicas

A read replica is a read-only copy of your primary database. It is useful for read-heavy workloads and takes the load-off your primary database.

> Exam Tip: Read replicas complement Multi-AZ deployments. The main purpose of read replicas is scalability, whereas the main purpose of Multi-AZ deployments is availability. However, you may use a read replica for disaster recovery of the source DB instance either in the same AWS region or in another region.

Each read replica has its own DNS endpoint. Read replicas can be promoted to be their own databases, however this breaks the replication to the primary database.

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

---
## DynamoDB Overview

---
## When Do We Use DynamoDB Transactions?

---
## Saving Your Data with DynamoDB Backups

---
## Taking Your Data Global with DynamoDB Streams and Global Tables

---
## Exam Tips

---
## Lab 8. Set Up a WordPress Site Using EC2 and RDS

<details>
<summary>Click here to start Lab 8.</summary>

### Introduction

</details>