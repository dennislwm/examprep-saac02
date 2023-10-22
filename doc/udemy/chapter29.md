# More Solution Architectures

<!-- TOC -->

- [More Solution Architectures](#more-solution-architectures)
    - [Event Processing in AWS](#event-processing-in-aws)
        - [SQS and Lambda](#sqs-and-lambda)
        - [SNS and Lambda](#sns-and-lambda)
        - [Fan Out with SNS and SQS](#fan-out-with-sns-and-sqs)
        - [S3 Event Notification](#s3-event-notification)
        - [External Event Notification](#external-event-notification)
    - [High Performance Computing HPC](#high-performance-computing-hpc)
        - [Data Management and Transfer](#data-management-and-transfer)
        - [Compute and Networking](#compute-and-networking)
        - [Storage](#storage)
        - [Automation and Orchestration](#automation-and-orchestration)
    - [AWS Prescriptive Guidance Patterns](#aws-prescriptive-guidance-patterns)
    - [Reference](#reference)

<!-- /TOC -->


---
## Event Processing in AWS

### SQS and Lambda

* Lambda polls SQS and retries a defined number of times to retrieve the message from SQS before it fails.
* Set up a Dead Letter Queue to allow failed messages to be removed from main queue to prevent blocking.

### SNS and Lambda

* SNS pushes a message asynchronously to Lambda.
* Lambda retries within its function before sending an error.
* Alternatively, Lambda can push the failed message to an SQS DLQ.

### Fan Out with SNS and SQS

* SNS pushes a message to multiple SQS queues.

### S3 Event Notification

* S3 Events can be sent to Lambda, EventBridge etc.
* EventBridge can then trigger multople destinations, such as Step Functions, Kinesis Data Streams etc.
  - Advanced filtering options with JSON rules.
  - Archive, replay events, reliable delivery.
  - Intercept any AWS API event via CloudTrail integration.

### External Event Notification

* API Gateway to receive external events.
* Send to Kinesis Data Stream and Data Firehose for processing and storing.
* Store events in S3 buckets as JSON files.

---
## High Performance Computing (HPC)

* Use cases are genomics, computational chemistry, financial risk modeling, weather prediction, ML, Deep Learning, autonomous driving etc.

### Data Management and Transfer

* Direct Connect.
  - Move data over a private secure network.
* Snowball and Snowmobile.
  - Move PBs of data to the cloud.
* DataSync
  - Move big data between On-Premises and S3, EFS, FSx for Windows.

### Compute and Networking

* EC2 Instances.
  - CPU optimized, GPU optimized.
  - Spot Instances / Fleets.
  - ASG.
* EC2 Placement Groups.
  - Cluster for good network performance.
* EC2 Enhanced Networking (SR-IOV).
  - Higher bandwidth, higher packer per second (PPS), lower latency.
  - Elastic Network Adapter (ENA) up to 100 GBps.
  - Intel 82599VF up to 10 GBps (legacy).
* EC2 Elastic Fabric Adapter (EFA).
  - Improved ENA for HPC, only works for Linux.
  - Great for inter-node communications, tightly coupled workloads.
  - Leverages Message Passing Interface (MPI) standard.
  - Bypasses the underlying Linux OS to provide low-latency, reliable transport.

### Storage

* Instance-attached Storage.
  - EBS scales up to 256,000 IOPS with `io2` Block Express.
  - Instance Store to scale to millions of IOPS, linked to EC2 instance, low latency.
* Network Storage.
  - S3 large blog, not a file system.
  - EFS scale IOPS based on total size, or use provisioned IOPS.
  - FSx for Lustre, HPC optimized distributed file system, millions of IOPS, backed by S3.

### Automation and Orchestration

* AWS Batch.
  - Supports multi-node parallel jobs, run single jobs that span multiple EC2 instances.
  - Easily schedule jobs and launch EC2 instances accordingly.
* AWS ParallelCluster.
  - Open-source cluster management tool to deploy HPC on AWS.
  - Configure with text files.
  - Automate creation of VPC, subnet, cluster type and instance types.
  - Ability to enable EFS on the cluster, improves network performance.

---
## AWS Prescriptive Guidance Patterns

AWS Prescriptive Guidance patterns provide step-by-step instructions, architecture, tools, and code for implementing specific cloud migration, modernization, and deployment scenarios.

You can use these patterns to move your on-premises or cloud workloads to AWS, regardless of whether you're in the proof of concept, planning, or implementation phase of your project.

The list of patterns by technical domain are not exhaustive below:

* Analytics
  - Build an ETL service pipeline to load data incrementally from S3 to Redshift using AWS Glue.
  - Generate test data using an AWS Glue job and Python.
  - Migrate Apache Cassandra workloads to Amazon Keyspaces using AWS Glue.
  - Migrate an on-premises Apache Kafka cluster to Amazon MSK by using MirrorMaker.
  - Orchestrate an ETL pipeline with validation, transformation, and partitioning using AWS Step Functions.
  - Access, query, and join DynamoDB tables using AWS Athena.
  - Subscribe a Lambda function to event notifications from S3 buckets in different AWS Regions.
  - Three AWS Glue ETL job types for converting data to Apache Parquet (to facilitate queries from AWS Athena).
  - Visualize Redshift audit logs using AWS Athena and QuickSight.
  - Visualize IAM credential reports for all AWS accounts using AWS QuickSight.
* Cloud-native
  - Build a video processing pipeline by using Kinesis Video Streams and Fargate.
  - Copy data from an S3 bucket to another account and Region by using the AWS CLI.
* Containers & Microservices
  - Access container applications privately on ECS by using AWS PrivateLink and a NLB.
  - Automate backups for RDS for PostgreSQL DB instances by using AWS Batch.
* Content Delivery
  - Send AWS WAF logs to Splunk by using AWS Firewall Manager and Kinesis Data Firehose.
  - Serve static content in an S3 bucket through a VPC by using CloudFront.
* Data Lakes
  - Configure cross-account access to a shared AWS Glue Data Catalog using AWS Athena.
* Databases
  - Block public access to RDS by using Cloud Custodian.
  - Copy DynamoDB tables across accounts by using AWS Backup.
  - Export RDS for SQL Server tables to an S3 bucket by using AWS DMS.
  - Implement cross-Region disaster recovery with AWS DMS and Amazon Aurora.
* Infrastructure
  - Check EC2 instances for mandatory tags at launch.
* Machine Learning & AI
  - Aggregate data in DynamoDB for ML forecasting in Athena.
  - Automatically extract content from PDF files using AWS Textract.

---
## Reference

* [AWS Prescriptive Guidance Patterns](https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/welcome.html)