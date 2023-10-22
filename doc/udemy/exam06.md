# Practice Exam 6

<!-- TOC -->

- [Practice Exam 6](#practice-exam-6)

<!-- /TOC -->ure in-flight data:
1. Capture data in an EC2 Instance store and then publish this data to Amazon Kinesis Data Firehose with S3 as the destination.

Does not have real-time or persistent storage capability:
1. Capture data in EBS volume and then publish this data to Amazon ElastiCache for Redis. Subscribe to the Redis channel to query the data.
2. Capture data in Kinesis Data Streams, and use Kinesis Data Analytics to query and analyze this streaming data in real-time.

**Question 6**

* Company has transitioned from using a single encryption key to multiple encryption keys by dividing the data into logical chunks.
* Move to S3 bucket and to encrypt each file with a different key.
* Implement this requirement without adding the overhead of splitting the data into logical groups.

**Question 7**

* Application uses a relational database that runs several queries that perform joins on multiple tables.
* The application needs to use a caching service that supports multi-threading.

**Question 10**

* Application lets users upload photos and perform image editing operations.
* The application offers two classes of service: pro and lite.
* Photos submitted by pro users to be processed before those submitted by lite users.
* Photos are uploaded to S3 and job information is sent to Amazon SQS.

**Question 14**

* Re-engineer the caching layer of its relational database.
* The caching layer must have replication and archival support built in.

**Question 19**

* Running a proprietary database on an EC2 instance using EBS volumes.
* The database is heavily I/O bound.
* Reecommend a RAID configuration that improves I/O performance.

**Question 20**

* Several provisioned throughput exceptions on its DynamoDB database due to major spikes in the writes to the database.
* Team wants to decouple the application layer from the database layer and dedicate a worker process to writing the data to DynamoDB.
* Recommend a middleware that can scale infinitely and cost-effective.

**Question 23**

* Flagged concerns over an EC2 instance querying IP addresses used for cryptocurrency mining.
* EC2 instance does not host any authorized application related to cryptocurrency mining.
* Protect the EC2 instance from unauthorized behaviour in the future.

**Question 24**

* Application is deployed on EC2 instances fronted by an ALB.
* Attackers perform over 100 requests per second, while your normal users only make about 5 requests per second.
* Efficiently prevent attackers from overwhelming your application.

**Question 25**

* Team uses Multi-AZ deployment for its MySQL RDS database to automate its database replication and augment data durability.
* Team has scheduled a maintenance window for a database engine level upgrade for the coming weekend.

**Question 27**

* Team wants to optimize its daily ETL process.
* Team migrates and transforms data from its S3 based data lake to a Redshift cluster.
* Team wants to manage this daily job in a serverless environment.

Serverless ETL within AWS cloud:
1. AWS Glue

Serverless ETL between On-premises and AWS cloud:
1. AWS Database Migration Service (DMS)

Non-serverless ETL:
1. AWS EMR
2. AWS Data Pipeline

**Question 29**

* Created a data warehouse using Redshift that is used to analyze data from Amazon S3.
* After 30 days, the data is rarely queried in Redshift.
* Preserve the SQL querying capability on your data and get the queries started immediately.
* Adopt a pricing model that can save the maximum cost on Redshift.

**Question 30**

* Moved it business critical data to EFS that will be accessed by multiple EC2 instances.
* Recommend access control such that only permitted EC2 instances can read from the EFS.

**Question 31**

* Set up a custom domain for internal usage such as `internal.example.com`.
* Use the private hosted zones feature of Route 53.
* Enable VPC settings.

**Question 36**

* Company wants to set up a HA architecture for a bastion host solution.

**Question 38**

* Application is hosted on multiple EC2 instances in the same AZ.
* Team wants to set up shared data access for these EC2 instances using EBS Multi-Attach volumes.

**Question 39**

* Team wants to orchestrate multiple ECS task types running on EC2 instances that are part of the ECS cluster.
* The output and state data for all tasks need to be stored.
* The amount of data output by each task is approximately 20 MB and there could be hundreds of tasks running at a time.
* As old outputs are archived, the storage size is not expected to exceed 1 TB.

**Question 41**

* Deploy a critical monolith application on a single web server as it hasn't been created to work in distributed mode.
* Team wants to set up automatic recovery from the failure of an AZ.

**Question 43**

* Team wants a secure connection between its on-premises and AWS cloud.
* The connection does not need high bandwidth and will handle a small amount of traffic.
* Team wants a quick turnaround time and minimal cost to set up the connection.

**Question 44**

* Application that uses a high-throughput request-response message pattern.
* Solution that saves development time and deployment costs.

**Question 51**

* Team has designed a VPC with a public subnet and a private subnet.
* The publicly accessible application will be hosted on several EC2 instances in an ASG.
* Team wants TLS termination to be offloaded from the EC2 instances.

**Question 64**

* Team has deployed a fleet of EC2 instances under an ASG.
* The instances span two AZs within the `us-east-1` region.
* Incoming requests are handled by an ALB that routes the requests to the EC2 instances under the ASG.
* As part of a test run, two instances belonging to AZ A were terminated.
* Later that day, one instance belonging to AZ B was detected as unhealthy by the ALB.