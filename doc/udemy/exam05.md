# Practice Exam 5

---
**Question 1**

* Terminated an instance in the `us-west-1a` AZ.
* EBS volume is available for attachment to other instances.
* Launch a new EC2 instance in the `us-west-1b` AZ but not possible to attach the EBS volume.
* Note: Limitation on sharing EBS volumes.

**Question 2**

* HA, HIPAA compliant, in-memory database that supports caching results of SQL queries.
* Note: Answers are all NoSQL databases.

Support for SQL query caching:
1. ElastiCache for Redis/Memcached.

Non-support for SQL query caching:
1. DocumentDB
2. DynamoDB DAX
3. DynamoDB

**Question 3**

* Write log files into S3, and read these in parallel on a near real-time basis.
* Address any data discrepancies when system overwrites an existing log file and tries to read that file.
* Note: Eventual consistency in S3.

**Question 4**

* Optimize AWS reesources across countries and regions.
* Understand best practices on cost optimization, performance, and security.
* Note: Use case of AWS resource.

**Question 5**

* EC2 instance is unable to connect to the internet using the IG.
* Note: Distractor in networking.

**Question 6**

* Video streaming provider is migrating to AWS Cloud.
* Supports at least a million requests per second for its EC2 server farm.
* Which type of ELB would you recommend?
* Note: Use case of ELB.

**Question 7**

* Archive 5PB of data in its on-premises data center to durable long term storage.
* Migrate this data in the most cost-optimal way.
* Note: Tranfer of large data into AWS cloud.

**Question 8**

* Deployed a microservice to ECS.
* Application layer is a Docker container that provides both static and dynamic content through an ALB.
* ECS cluster is experiencing higher network usage, with 90% of it due to static content.
* Improve network usage and decrease cost.

**Question 9**

* Created a cost-effective backup solution in another AWS Region.
* Application running in warm standby mode behind an ALB.
* Current failover process is manual and requires updating the DNS alias record to point to a secondary ALB in another region in case of failure of the primary ALB.
* Automate the failover process.
* Note: Cross-Region ALB.

**Question 10**

* EC2 contains sensitive data.
* Consider using WAF to protect data.
* Note: WAF integration with other AWS services.

WAF can be deployed on:
* CloudFront
* ALB
* API Gateway

WAF cannot be deployed on:
* EC2 instance

Prevent external cyber-attacks with WAF:
1. CloudFront

Prevent both internal and external cyber-attacks with WAF:
1. ALB
2. API Gateway

**Question 11**

* Performance has deteriorated after a new ASG was deployed.
* Launch configuration for the ASG is using the incorrect instance type.
* Provide a long term resolution.
* Note: Limitations of Launch configurations.

**Question 12**

* News agency upload and download video files of 500MB each to and from S3 bucket daily.
* Poor latency for accessing the S3 bucket in remote locations.
* Continue using serverless storage solution but wants to improve performance.

Speed up downloads of video files:
* CloudFront
* S3 Transfer Acceleration

Speed up uploads of video files:
* CloudFront
* S3 Transfer Acceleration

Serverless solution to speed up transfer of video files:
1. CloudFront
2. S3 Transfer Acceleration
3. Create new buckets in every region

Non-serverless solution to speed up transfer of video files:
1. Spin up EC2 instances in each region, and create a daily job to transfer S3 data into EBS volumes.
2. Move S3 data into EFS created in a US region, connect to EFS from EC2 instances in other regions using VPC peering.

**Question 13**

* Use Route53 to configure DNS records for the primary website.
* Domain pointing to the ALB.
* Users will be redirected to a static error page, in case of unavailability of the primary website.
* Meet the company's requirement, while keeping changes to a bare minimum.
* Note: Route53 routing policy.

**Question 14**

* Store business-critical data on EBS volumes which provide persistent storage.
* On terminating an EC2 instance, the attached EBS volume was lost.

**Question 15**

* Three-tier architecture composed of an ALB, ASG-managed EC2 instances, and an Aurora database.
* Adhere to the security pillar of the well-architected framework.
* Configure the security group of the Aurora database to allow traffic from EC2 instances.

**Question 16**

* Establish encrpyted network connectivity between its on-premises and AWS cloud.
* Up and running in the fastest possible time.
* Support encryption in transit.

**Question 17**

* Application generates a large number of files on S3, each 10 MB in size.
* Files must be stored for 5 years before they can be deleted.
* Files are frequently accessed in the first 30 days.
* Files are rarely accessed after the first 30 days.
* Files are not easily reproduce, and must be immediately accessible when required.

**Question 18**

* Ensure HA for RDS database.
* What happens when the primary instance of the Multi-AZ RDS goes down.

AWS managed failover steps of RDS primary instance:
1. The CNAME record for your DB instance will be updated to point to the standby DB.

Non-failover steps of RDS primary instance:
1. An email will be sent to the SA asking for manual intervention.
2. The URL to access the database will change to the standby DB.
3. The application will be down until the primary database has recovered itself.

**Question 19**

* Company maintains a Direct Connect connection to AWS cloud.
* Recently migrated its data warehouse to AWS.
* Analyst wants to query the data warehouse using a visualization tool.
* Query responses returned by the data warehouse are not cached in the visualization tool.
* Egress data is 60 MB on average per query from data warehouse.
* Egress data is 600 KB on average per webpage from visualization tool.
* Lowest data transfer egress cost.

* Data transfer pricing over Direct Connect is lower than over the internet.
* Data warehouse to visualization tool is 60 MB on average and should be on AWS.

Visualization tool over Direct Connect or internet in a different region:
1. Deploy the visualization tool on-premises. Query the data warehouse over the internet at a location in the same AWS region.
2. Deploy the visualization tool on-premises. Query the data warehouse over Direct Connect at a location in the same AWS region.
3. Deploy the visualization tool in the same AWS region as the data warehouse. Access the visualization tool over the internet at a location in the same region.

Visualization tool over Direct Connect in the same AWS region:
1. Deploy the visualization tool in the same AWS region as the data warehouse. Access the visualization tool over Direct Connect at a location in the same region.

**Question 20**

* Use SQS queues to decouple the application architecture.
* Message processing fails for some customer orders.
* Note: Dead-letter queue.

**Question 21**

* Create a static website and deployed it on S3.
* Endpoint for the static website.

**Question 22**

* Users report frequent sign-in requests from the application.
* Application is hosted on multiple EC2 instances behind ALB.
* Root cause is unhealthy servers causing session data to be lost.
* Implement a distributed in-memory cache-based session management solution.

**Question 23**

* Production database is on Aurora MySQL DB cluster.
* Team wants access to multiple test databases that must be re-created from production data.
* Create these test databases quickly with the least effort.

Clone or promote Aurora DB easily:
1. Use database cloning to create multiple clones of the production DB and use each clone as a test DB.

Not clone nor promote Aurora DB easily:
1. Create additional Read Replicas of the Aurora MySQL production DB and use the RR for testing by promoting each Replica to be its own independent standalone instance.
2. Enable database Backtracking on the production DB and let the testing team use the production DB.
3. Take a backup of the Aurora MySQL DB instance using the mysqldump utility, create multiple new test DB instances and restore each test DB from the backup.

**Question 24**

* Debug issues related to EC2 instance.
* SSH into instance to retrieve the instance public IP from a shell command.
* Note: Instance metadata.

**Question 25**

* Design a low-latency solution for a static, single-page application.
* Access the page through a custom domain name.
* Serverless, encrypted in transit, cost effective.

**Question 26**

* Chat application uses DynamoDB.
* CloudTrail enabled.
* Review the configuration settings for DynamoDB.
* DynamoDB encryption details cannot be found.
* Note: DynamoDB configuration.

Non-support for AWS owned CMK:
1. View
2. Use
3. Track
4. Audit

DynamoDB default encryption:
1. AWS owned CMK

DynamoDB non-default encryption:
1. Customer managed CMK

Not supported DynamoDB encryption:
1. AWS managed CMK
2. Data keys

**Question 27**

* Publish an event into an SQS queue whenever a new object is upload to S3.
* Note: Limitation of S3 to SQS integration.

Supported S3 to SQS integration:
1. Standard SQS queue

Non-supported S3 to SQS integration:
1. FIFO SQS queue

**Question 28**

* Application with global users across AWS Regions.
* ELB in a Region malfunctioned and traffic was down.
* Reduce internet latency and add automatic failover of ELB across AWS Regions.

Automatic failover of ELB across AWS Regions:
1. AWS Global Accelerator

Non-automatic failover of ELB across AWS Regions:
1. Route 53 geoproximity routing policy
2. Direct Connect
3. CloudFront

**Question 29**

* Accelerated online migration of 100 TB from on-premises to S3.
* Establish a mechanism to access the S3 data.
* Most performance.

Support ongoing updates from on-premises:
1. File Gateway

Non-support ongoing updates from on-premises:
1. DataSync
2. S3 Transfer Acceleration

**Question 30**

* Running batch jobs on EC2 instances with EBS volumes attached.
* Meet HIPAA compliance for sensitive data stored on EBS volumes.
* Encrpytion of EBS volumes.

Data is encrypted:
* At rest inside EBS volume.
* Any snapshot created from EBS volume.
* Data moving between EBS volume and EC2 instance.
* Volumes created from snapshots.

**Question 31**

* Use a Lambda function to write order data into a single DB instance Aurora cluster.
* Many order writes getting missed during peak load times.
* DB has experienced high CPU and memory consumption during peak times.
* Enhance the availability of the Aurora DB.

* There are no standby instances in Aurora, only read replica instances.
* You cannot manually promote a read replica, Aurora will automatically promote a read replica if the primary instance fails.

**Question 32**

* Block access to its application from specific countries.
* Allow its remote team to access the application from one of the blocked countries.
* Application is deployed on EC2 instances behing and ALB with WAF.

**Question 33**

* Application deployed with ELB and ASG.
* Configure CloudWatch alarms to make ASG scale in and out quickly.
* Production bug appears two days, but team unable to SSH into instance due to ASG terminating the instance.
* Log files aare saved on EC2 instance.

**Question 34**

* Cross-zone load balancing for ALB vs NLB.
* Note: Default settings.

Default enabled cross-zone:
1. ALB

Default disabled cross-zone:
1. NLB

**Question 35**

* Workflow to ingest the clickstream data into S3 data lake.
* Run some SQL based query on data.

**Question 36**

* Signed contracts must be encrypted using the AES-256 algorithm.
* Encryption key is generated internally.
* Migrating to AWS cloud.
* Data to be encrypted on AWS.
* Continue using their existing encryption key generation mechanism.
* Note: SSE shorthand notations.

**Question 37**

* Using Amazon Redshift.
* Move any historical data older than a year into S3.
* Retain the ability to cross-reference this historical data.
* Least amount of effort and minimal cost.

High cost to load and query in Redshift:
1. Glue ETL
2. Redshift COPY
3. Athena

Low cost to query in Redshift:
1. Redshift Spectrum

**Question 38**

* Web application on EC2 instances behind ASG.
* Critical application.
* Workload can be managed on two instances and peak up to 6 instances.

**Question 39**

* Critical application using a single Spread placement group of EC2 instances.
* Needs 15 EC2 instances for optimal performance.
* How many AZs required to deploy these EC2 instances.

**Question 40**

* Moving to AWS cloud.
* Enforce data protection on S3 to meet compliance.

S3 encryption:
1. Data at rest using client-side encryption, i.e. user encryption.
2. Data at rest using server-side encryption, i.e. server-side S3-managed (SSE-S3), server-side AWS KMS (SSE-KMS), server-side customer-managed (SS3-C).
3. Data in transit using HTTPS (TLS).

S3 non-encryption:
1. Object metadata

**Question 41**

* Two-tier architecture with application in public subnet and RDS MySQL DB in private subnet.
* Use bastion host in the public subnet to access the RDS.
* Application error "could not connect to server: connection timed out".

**Question 42**

* Fleet of EC2 instances behind ALB for video streaming application.
* Created a CloudFront distribution with the ALB as custom origin.
* Spike in the number and types of SQL injection and cross-scripting attack vectors on the application.
* Countering these attacks.

**Question 43**

* Microservice approach to expose the website from the same LB, linked to different TGs.
* Each TG has a different URL based on the same domain name.

**Question 44**

* Optimize I/O bound processes on EC2 instances.
* Ideal storage with high-performance IOPS.
* Temporary storage space.

High-random temporary I/O performance:
1. Instance Store

High-random permanent I/O performance:
1. EBS General Purpose SSD (`gp2`)

High persistent permanent I/O performance:
1. EBS Provisioned IOPS SSE (`io1`)

Low persistent permanent I/O performance:
1. EBS Throughput Optimized HDD (`st1`)

**Question 45**

* Mobile users to connect through Google login.
* MFA capability.
* Fully managed AWS solution.

**Question 46**

* EC2 server fleet behind ALB and ASG.
* Anticipate huge traffic spike for upcoming sale.
* Leverage ASG feature to pre-empt surge in traffic.

**Question 47**

* Scalable near-real-time to share data with multiple internal applications.
* Must remove sensitive data from data before storage.
* Store data in a document database for low-latency retrieval.

**Question 48**

* User added to admin group of AWS account.
* The group has an attached `AdministratorAccess` managed policy.

AWS tasks that require `root`:
1. Close the AWS account.
2. Configure an S3 bucket to enable MFA delete.

AWS tasks that do not require `root`:
1. Delete the IAM user.
2. Delete an S3 bucket.
3. Change the password for IAM user account.

**Question 49**

* Build a video streaming product with ALB and EC2 instances.
* ALB removes an instance from its pool of healthy instances whenever it is detected as unhealthy.
* ASG fails to provision the replacement instance.

Support EC2 health check:
1. ASG

Non-support EC2 health check:
1. ALB

Support ALB health check:
1. ASG
2. ALB

**Question 50**

* Stores patient health records on S3.
* Implement an archival solution on S3 Glacier.
* Note: S3 vault.

Support S3 Glacier compliance:
1. vault lock policy

Non-support S3 Glacier compliance:
1. Access Control List
2. lifecycle policy

**Question 51**

* Move to AWS cloud.
* Automate solution to analyze customer service calls using sentiment analysis.
* Ad-hoc SQL queries.

Support audio files:
1. Transcribe

Non-support audio files:
1. Kinesis Data Streams

Support SQL queries:
1. Athena

Non-support SQL queries:
1. Quicksight

**Question 52**

* Retain activity logs for each customer for compliance.
* Retain the logs for 5-10 years.
* Data size is expected to be in PBs.
* Accessible within a timeframe of up to 48 hours.

Accessible within 48 hours:
1. Glacier
2. Glacier Deep Archive

**Question 53**

* IoT company wants to measure key metrics from its smart devices.
* Streaming system that supports ordered data based.
* Sustains high throughput messages (thousands of messages per second).

**Question 54**

* Moving to AWS cloud.
* Needs 10 TB storage with maximum I/O performance.
* Close to 450 TB of durable storage.
* Plus 900 TB for archival.

**Question 55**

* IoT company wants a DB solution wwith auto scaling and HA.
* DB must handle any changes in data attributes over time.
* DB must provide capability to output a continuous stream.

**Question 56**

* Enhance relational DB performance.
* Looking for a caching solution with support for geospatial data.

**Question 57**

* Kinesis Data Streams to process IoT data.
* Multiple consumers are using the incoming data streams.
* Performance lag between producers and consumers of data streams.
* Improve performance of data streams.

**Question 58**

* Maintains 5 different VPCs for resource isolation.
* Wants to interconnect all VPCs.
* Set up VPC peering connections between VPC A and all other VPCs.
* Failed to establish connectivity between all VPCS.
* Most resource-efficient and scalable solution.

**Question 59**

* Manage mobile apps to capture and send data to Kinesis Data Streams.
* Error `ProvisionedThroughputExceededException`.
* Messages are being sent one by one at a high rate.

Increase Kinesis Data Streams throughput with high costs:
1. Increase the number of shards.

Increase Kinesis Data Streams throughput with low costs:
1. Use batch messages.

Not increase throughput:
1. Use Exponential Backoff.
2. Decrease the Stream retention duration.

**Question 60**

* Manages three EC2 instances.
* Read-heavy DB requests to RDS for PostgreSQL.
* Make the RDS resilient from a Disaster Recovery.

**Question 61**

* VPC with two public and two private subnets.
* Deploy ALBs in public subnets.
* Deploy EC2 instances in private subnets.
* Wants EC2 instances to have access to the internet.
* Fully managed by AWS and works over IPv4.

**Question 62**

* Create a REST API using serverless.

**Question 63**

* Massive PostgreSQL database.
* Retain control over managing patches.
* Install the database on EC2 instance with optimal storage type.
* High IOPS.

**Question 64**

* Use S3 bucket to store critical data.
* Hundreds of S3 buckets.
* Lifecycle policies on S3 buckets sub-optimal.
* Reduce storage costs.

**Question 65**

* Application deployed on EC2 instance.
* Bug causes instance to freeze regularly.
* Manually restart instance via AWS console.
* Cost-optimal and resource-efficient automated solution.