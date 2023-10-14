# Chapter 22. Data & Analytics

---
## Athena Performance

* Use columnar data for cost savings.
  - Use Glue to convert your data to Apache Parquet or ORC format.
* Compress data for smaller retrievals.
* Partition data in S3 for easy querying, e.g. `s3://mybucket/flight/parquet/year=1991/month=1`.
* Use a large data file (> 128 Mb) to minimize overhead.
* Allows you to run SQL queries across custom data sources On-Premises and AWS cloud.
  - To run Federated Queries, use Data Source Connector that run on Lambda functions.
  - Query custom data sources, such as ElastiCache, DocumentDB, DynamoDB, CloudWatch Logs, RDS, On-Premises Databases etc.
* Queried data and results are stored in an S3 bucket.
* Use QuickSight as a dashboard for analyzed data stored in the S3 bucket.
* Use Redshift instead of Athena for creating indexes, and querying data with multiple joins.
* Use Athena instead of Redshift for query data without loading into tables.
  - If you have Redshift already, then use Redshift Spectrum for query data without loading into tables.
* Use Athena instead of Redshift for serverless solution

---
## Redshift

### Loading Data into Redshift

* Amazon Kinesis Data Firehose (through S3 COPY).

* Manual S3 COPY.
  - through internet
  - through VPC with enhanced VPC Routing

* EC2 Instance through JDBC Driver.

### Redshift Spectrum

* Query structured and semi structured data from S3 without having to load the data into Redshift tables.
* Employ parallelism to run SQL queries, filtering, joins and aggregation against large datasets.
* Fully managed including automatic scaling and high availability.
* Redshift Spectrum cluster and S3 bucket must be in the same Region.
* The database user must have permission to create temporary tables in the database.
* Does not support EMR with Kerberos.

---
## OpenSearch Pattern

Use OpenSearch to complement DynamoDB with full or partial search on any fields, as DynamoDB is limited to searching on keys and indexes only.

To enable OpenSearch on DynamoDB.
- Create a DynamoDB Stream to get data from DynamoDB and send it to a Lambda function.
- The Lambda function ingests data and convert to OpenSearch.
- Use OpenSearch API to query data and return a valid key.
- Use the DynamoDB API to retrieve values of the valid key.

To enable OpenSearch on CloudWatch Log.
- Create a Subscription Filter to get data from CloudWatch Log and send it to a Lambda function.
- The Lambda function ingests data and convert to OpenSearch.
- Alternatively, use Kinesis Data Firehose instead of Lambda to ingests data and convert to OpenSearch.

---
## AWS Lake Formation

AWS is a fully managed service to set up a Data Lake, which is a central place to have all your data for analytics purposes.

* Automates complex manual steps for collecting, cleansing, moving, cataloging, deduplicating data.

* Discover, combine, transform, and ingest both structured and unstructured data from On-Premises or AWS Cloud.

* Blueprints for S3, RDS, NoSQL etc.

* Centralized permissions for all data analytics.

* Fine grained access control for your applications.

* Underlying Glue technology for ETL.

---
## Big Data Ingestion Pipeline

* Real time data.
  - Ingest data into Kinesis Data Streams.
  - Frequently update data into supported storage, such as S3 etc, using Kinesis Data Firehose.
  - Attach a Lambda function to Kinesis Data Firehose for cleansing data.

---
## Reference

* [Querying external data using Amazon Redshift Spectrum](https://docs.aws.amazon.com/redshift/latest/dg/c-using-spectrum.html)