# Chapter 14. Big Data

---
## Exploring Large Redshift Databases

AWS Redshift is just a large relational RDS, but is suitable for:

* Volume - ranges from TBs to PBs of data, up to 16 PBs.

* Variety - includes data from a wide range of sources and formats.

* Velocity - data needs to be collected, store, processed, and analyzed within a short period of time.

* BI - suitable for Business Intelligence applications.

---
## Processing Data with Elastic MapReduce (EMR)

Elastic MapReduce (EMR) is a managed big data platform that allows you to process (extract, transform and load) vast amounts of data using open-source tools, such as Spark, Hive, HBase, Flink, Hudi and Presto. EMR is an AWS ETL service that manages the underlying infrastructure cluster, i.e. a fleet of instances, for your selected open-source tool.

You can use Reserved Instances to reduce your costs, and the cluster lives inside your VPC.

---
## Streaming Data with Kinesis

Kinesis allows you to **ingest, process, and analyze real-time streaming data**. There are two types of Kinesis:

* Data Streams - real time, but you're responsible for creating the consumer and scaling.

* Data Firehose - near real time (within 1 minute), however AWS manages the architecture.

### Kinesis Data Streams

* Producer - any resource that produces the real-time streaming data.

* Consumer - ingest the process data and sends it to any endpoint within AWS.

![Example Diagram](../../img/acloudguru/Chp14.1.png)

### Kinesis Data Firehose

![Example Diagram](../../img/acloudguru/Chp14.2.png)

### Kinesis Data Analytics and SQL

Kinesis Data Analytics can be integrated with either Data Streams or Data Firehose to analyze data using standard SQL.

* Easy - integrates with your Kinesis pipeline.

* Serverless - fully managed, real-time serverless service, which handles scaling and provisioning of resources.

* Cost - PAYG you only pay for the amount of resources you consume as your data passes through.

* Real-time - real-time processing that offers speed.

* Transform - apply any SQL procedure on the data passing through.

### Kinesis vs SQS

|       Kinesis       |           SQS           |
|:-------------------:|:-----------------------:|
| Real-time messaging | No real-time messaging  |
|  More complicated   |      Simple to use      |
|   Transform data    | Does not transform data |

---
## Amazon Athena and AWS Glue

Athena is a serverless interactive query service that makes it **easy to analyze data in S3 using SQL**. This allows you to directly query data in your S3 bucket without loading it into a database.

AWS Glue is a serverless data integration service that makes it easy to discover, prepare, and combine data. It allows you to **perform ETL workloads without managing underlying servers**.

### Combining Athena and Glue

You can put AWS Glue in between S3 bucket and Athena to perform ETL on the data in S3, before analyzing it with Athena using SQL. You can send the SQL results to QuickSight to create and share internal dashboards.

![Example Diagram](../../img/acloudguru/Chp14.3.png)

---
## AWS QuickSight

AWS QuickSight is fully managed business intelligence (BI) data visualization service, similar to Tableau. It allows you to easily **create dashboards and share them within your company**.

You can put AWS QuickSight after Athena to create and share internal dashboards to your team and company.

---
## Moving Transformed Data Using AWS Data Pipeline

AWS Data Pipeline is a managed Extract, Transform, Load (ETL) servoce for automating movement and transformation of your data. Data Pipeline is AWS native ETL service, which is a competitor to open-source ETL tools, such as Spark etc, that are managed by AWS Elastic MapReduce (EMR).

* Data Driven - define data-driven workflows, and automate these steps that are dependent on previous tasks completing successfully.

* Parameters - define your parameters for data transformations, and enforces your chosen logic.

* Highly Available - managed service that is HA and fault tolerant.

* Fault Tolerant - automatically retries failed tasks, and notifications can be sent via SNS.

* Storage - integrates with DynamoDB, RDS, Redshift and S3.

* Compute - works with EC2 instances and EMR for compute needs.

### Data Pipeline Components

* Pipeline Definition - specify the business logic of your data management needs.

* Managed Compute - service will create EC2 instances to perform your activities.

* Task Runners - task runners within instances poll for different tasks and perform them when found.

* Data Nodes - define locations and types of data that will be input and output.

* Activities - activities are pipeline components that define the work to perform.

### Data Pipeline Use Cases

* Processing data in EMR using Hadoop streaming.

* Importing or exporting DynamoDB data.

* Copying CSV files or data between S3 buckets.

* Exporting RDS data to S3.

* Copying data to Redshift.

---
## Implement Amazon Managed Streaming for Apache Kafka (Amazon MSK)

Amazon MSK is fully managed service for running data streaming applications that uses Apache Kafka.

* Control Plane - provides control plane operations, such as create, update and delete clusters.

* Data Plane - leverage Kafka data plane operations for producing and consuming streaming data.

* Existing Applications - open-source versions of Kafka allow support for existing apps, tools, and plugins.

* Serverless - fully compatible with Kafka for automatic provisioning and scaling.

* Connect - allows developers to easily stream data to and from Kafka clusters.

### MSK Components and Concept

* Broker Node - specify the number of broker nodes per AZ at time of cluster creation.

* ZooKeeper Node - these nodes are created for you.

* Producers, Consumers and Topics - data plane operations allow creation of topics and ability to produce and consume data.

* Flexible Control Plane - perform cluster operations with the AWS console, CLI, or APIs within any SDK.

### Resiliency in MSK

* Automatic Recovery - automatic detection and recovery from common failure scenarios.

* Detection - detected broker failures result in mitigation or replacement of unhealthy nodes.

* Reduce Data - tries to reuse storage from older brokers during failures to reduce data needing replication.

* Time Required - impact time is limited to however long it takes MSK to complete detection and recovery.

* After Recovery - after a successfuly recovery, producer and consumer apps continue to communicate with the same broker IP as before.

### Security and Logging

* Integration - integrate with KMS for SSE requirements.

* Encryption - encrypted at rest by default.

* Transit - TLS 1.2 for encryption in transit between brokers in clusters.

* Logs - deliver broker logs to CloudWatch, S3 and Kinesis Data Firehose.

* Metrics - metrics are gathered and sent to CloudWatch.

* CloudTrail - all MSK API calls are logged to CloudTrail.

---
## Analyzing Data with Amazon OpenSearch Service

OpenSearch is a managed service allowing you to **run search and analytics engines for various use cases**. It is the successor to Amazon ElasticSearch service.

* Quick Analysis - quickly ingest, search, and analyze data in your clusters. Commonly part of an ETL process.

* Scalable - easily scale cluster infrastructure running the open-source OpenSearch services.

* Security - leverage IAM for access control, VPC security groups, encryption at rest and in transit, and field-level security.

* Stability - multi-AZ capable service with master nodes and automated snapshots.

* Flexible - allows for SQL support for BI apps.

* Integration - easily integrate it with CloudWatch, CloudTrail, S3 and Kinesis.

### Exam Tips

* In the exam, if the question talks about creating a logging solution involving visualization of log file analytics or BI reports, think of OpenSearch (or ElasticSearch).