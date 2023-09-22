# Practice Exam 1

---
## Domain 1: Design Secure Architectures

**Question 11**

You work in healthcare for an IVF clinic. You host an application on AWS, which allows patients to track their medication during IVF cycles. The application also allows them to view test results, which contain sensitive medical data. You have a regulatory requirement that the application is secure and you must use a firewall managed by AWS that enables control and visibility over VPC-to-VPC traffic and prevents the VPCs hosting your sensitive application resources from accessing domains using unauthorized protocols. What AWS service would support this?

* (Selected) AWS WAF

AWS WAF is a web application firewall that helps protect web applications from attacks by allowing you to configure rules that allow, block, or monitor (count) web requests based on conditions that you define. These conditions include IP addresses, HTTP headers, HTTP body, URI strings, SQL injection and cross-site scripting.

* (Incorrect) AWS Firewall Manager

* (Correct) AWS Network Firewall

The AWS Network Firewall infrastructure is managed by AWS, so you don’t have to worry about building and maintaining your own network security infrastructure. AWS Network Firewall’s stateful firewall can incorporate context from traffic flows, like tracking connections and protocol identification, to enforce policies such as preventing your VPCs from accessing domains using an unauthorized protocol. AWS Network Firewall gives you control and visibility of VPC-to-VPC traffic to logically separate networks hosting sensitive applications or line-of-business resources.

* (Incorrect) AWS PrivateLink

**Question 59**

You are managing S3 buckets in your organization. One of the buckets in your organization has gotten some bizarre uploads and you would like to be aware of these types of uploads as soon as possible. Because of that, you configure event notifications for this bucket. Which of the following is NOT a supported destination for event notifications?

* (Correct) SES

Correct. SES is a NOT supported destination for S3 event notifications. The Amazon S3 notification feature enables you to receive notifications when certain events happen in your bucket. To enable notifications, you must first add a notification configuration that identifies the events you want Amazon S3 to publish and the destinations where you want Amazon S3 to send the notifications. Amazon S3 can send event notification messages to the following destinations. You specify the ARN value of these destinations in the notification configuration.

Publish event messages to an Amazon Simple Notification Service (Amazon SNS) topic
Publish event messages to an Amazon Simple Queue Service (Amazon SQS) queue Note that if the destination queue or topic is SSE enabled, Amazon S3 will need access to the associated AWS Key Management Service (AWS KMS) customer master key (CMK) to enable message encryption.
Publish event messages to AWS Lambda by invoking a Lambda function and providing the event message as an argument

* (Selected) SNS

Incorrect. You can publish event messages to an Amazon Simple Notification Service (Amazon SNS) topic.

* (Incorrect) Lambda function

* (Incorrect) SQS

---
## Domain 2: Design Resilient Architectures

**Question 12**

Your development team has created a gaming application that uses DynamoDB to store user statistics and provide fast game updates back to users. The team has begun testing the application but needs a consistent data set to perform tests with. The testing process alters the dataset, so the baseline data needs to be retrieved upon each new test. Which AWS service can meet this need by exporting data from DynamoDB and importing data into DynamoDB?

* (Selected) AWS Import/Export

Incorrect. AWS Import/Export is a service you can use to transfer large amounts of data from physical storage devices into AWS. You mail your portable storage devices to AWS, and AWS Import/Export transfers data directly off of your storage devices using Amazon's high-speed internal network.

* (Correct) Elastic Map Reduce

You can use Amazon EMR with a customized version of Hive that includes connectivity to DynamoDB to perform operations on data stored in DynamoDB:

Loading DynamoDB data into the Hadoop Distributed File System (HDFS) and using it as input into an Amazon EMR cluster
Querying live DynamoDB data using SQL-like statements (HiveQL)
Joining data stored in DynamoDB and exporting it or querying against the joined data
Exporting data stored in DynamoDB to Amazon S3
Importing data stored in Amazon S3 to DynamoDB

* (Incorrect) Redshift

* (Incorrect) DAX

**Question 15**

You are working for a large financial institution and have been tasked with creating a relational database solution to deal with a read-heavy workload. The database needs to be highly available within the Oregon region and quickly recover if an Availability Zone goes offline. Which of the following would you select to meet these requirements?

* (Correct) Create a read replica and point your read workloads to the new endpoint RDS provides.

Amazon RDS uses the MariaDB, MySQL, Oracle, PostgreSQL, and Microsoft SQL Server DB engines' built-in replication functionality to create a special type of DB instance called a read replica from a source DB instance. Updates made to the source DB instance are asynchronously copied to the read replica. You can reduce the load on your source DB instance by routing read queries from your applications to the read replica. Using read replicas, you can elastically scale out beyond the capacity constraints of a single DB instance for read-heavy database workloads.

* (Incorrect) Use an Amazon Aurora global database to ensure a region failure won't break the application.

* (Incorrect) Split your database into multiple RDS instances across different regions. In the event of a failure, point your application to the new region.

* (Correct) Enable Multi-AZ support for the RDS database.

Multi-AZ creates a secondary database in another AZ within the region you are in. If something were to happen to the primary database, RDS would automatically fail over to the secondary copy. This allows your database achieve high availability with minimal work on your part. https://aws.amazon.com/rds/features/multi-az/

* (Incorrect) Using RDS, create a read replica. If an AZ fails, RDS will automatically cut over to the read replica.

Read replicas can be promoted to become a primary database, but this is a manual procedure. Amazon RDS uses the MariaDB, MySQL, Oracle, PostgreSQL, and Microsoft SQL Server DB engines' built-in replication functionality to create a special type of DB instance called a read replica from a source DB instance. Updates made to the source DB instance are asynchronously copied to the read replica. You can reduce the load on your source DB instance by routing read queries from your applications to the read replica. Using read replicas, you can elastically scale out beyond the capacity constraints of a single DB instance for read-heavy database workloads.

**Question 50**

Recently, a production instance was forced to reboot unexpectedly during business hours, causing significant customer impact. After root cause analysis, it was found that the reboot was caused by AWS due to underlying hardware maintenance that needed to be performed. You have been tasked with finding a way to automate the start and stop of the Amazon EC2 instance in case of similar future events. How would you go about creating the optimal solution for this?

* (Incorrect) Set up AWS Health Scheduled Events to automatically reboot the Amazon EC2 instance during night hours.

* (Selected) Set up an Amazon EventBridge rule that is triggered by the AWS Health event. Target a Lambda function to parse the incoming event and reference the Amazon EC2 instance, ID included. Have the function perform a stop and start of the instance.

AWS Health provides events for ongoing visibility into your AWS accounts, resources, and even public services. EventBridge can be enabled to detect incoming health events and then trigger a rule targeting downstream resources, like AWS Lambda. These types of health events require you to start and then stop EC2 instances to move them between underlying hosts. Reference: What is AWS Health? Monitoring AWS Health events with Amazon EventBridge

* (Incorrect) Use AWS CloudTrail to kick off a reboot event in AWS Health based on the EC2 instance ID received.

* (Incorrect) You cannot automate this. Instead, set up an Amazon SNS notification topic to alert operations teams that they need to log in and manually stop and then start the Amazon EC2 instance.

---
## Domain 3: Design High-Performing Architectures

**Question 5**

Your boss has tasked you with decoupling your existing web frontend from the backend. Both applications run on EC2 instances. After you investigate the existing architecture, you find that (on average) the backend resources are processing about 50,000 requests per second and will need something that supports their extreme level of message processing. It's also important that each request is processed only 1 time. What can you do to decouple these resources?

* (Incorrect) Upsize your EC2 instances to reduce the message load on the backend servers.

* (Correct) Use SQS Standard. Include a unique ordering ID in each message, and have the backend application use this to deduplicate messages.

This would be a great choice, as SQS Standard can handle this level of extreme performance. If the application didn't require this level of performance, then SQS FIFO would be the better and easier choice.

* (Selected) Use SQS FIFO to decouple the applications.

While this would seem like the correct answer at first glance, it's important to know SQS FIFO has a batch limit of 3,000 messages per second and can't handle the extreme level of performance that's required in this situation. https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/quotas-messages.html

* (Incorrect) Use S3 to store the messages being sent between the EC2 instances.

**Question 23**

You have just started work at a small startup in the Seattle area. Your first job is to help containerize your company's microservices and move them to AWS. The team has selected ECS as their orchestration service of choice. You've discovered the code currently uses access keys and secret access keys in order to communicate with S3. How can you best handle this authentication for the newly containerized application?

* (Incorrect) Leave the credentials where they are.

* (Incorrect) Attach a role to the EC2 instances that will run your ECS tasks.

* (Selected) Attach a role with the appropriate permissions to the task definition in ECS.

It's always a good idea to use roles over hard-coded credentials. One of the best parts of using ECS is the ease of attaching roles to your containers. This allows the container to have an individual role even if it's running with other containers on the same EC2 instance.

* (Incorrect) Migrate the access and secret access keys to the Dockerfile.

**Question 34**

A large, big-box hardware chain is setting up a new inventory management system. They have developed a system using IoT sensors which captures the removal of items from the store shelves in near real-time and want to use this information to update their inventory system. The company wants to analyze this data in the hopes of being ahead of demand and properly managing logistics and delivery of in-demand items.

Which AWS service can be used to capture this data as close to real-time as possible, while being able to both transform and load the streaming data into Amazon S3 or Elasticsearch?

* (Correct) Amazon Kinesis Data Firehose

Amazon Kinesis Data Firehose is the easiest way to reliably load streaming data into data lakes, data stores, and analytics tools. It can capture, transform, and load streaming data into Amazon S3, Amazon Redshift, Amazon Elasticsearch Service, and Splunk, enabling near-real-time analytics with existing business intelligence tools and dashboards you’re already using today. It is a fully-managed service that automatically scales to match the throughput of your data and requires no ongoing administration. It can also batch, compress, transform, and encrypt the data before loading it, minimizing the amount of storage used at the destination and increasing security.

* (Incorrect) Amazon Redshift

* (Incorrect) Amazon Aurora

* (Selected) Amazon Kinesis Data Streams

Amazon Kinesis Data Streams is a scalable and durable real-time data streaming service. Amazon Kinesis Firehose serves as a data transfer service. Kinesis Firehose is the most suitable option for loading streaming data to Amazon S3 and Elasticsearch.

**Question 36**

You work for an oil and gas company as a lead in data analytics. The company is using IoT devices to better understand their assets in the field (for example, pumps, generators, valve assemblies, and so on). Your task is to monitor the IoT devices in real-time to provide valuable insight that can help you maintain the reliability, availability, and performance of your IoT devices. What tool can you use to process streaming data in real time with standard SQL without having to learn new programming languages or processing frameworks?

* (Correct) Kinesis Data Analytics

Monitoring IoT devices in real-time can provide valuable insight that can help you maintain the reliability, availability, and performance of your IoT devices. You can track time series data on device connectivity and activity. This insight can help you react quickly to changing conditions and emerging situations. Amazon Web Services (AWS) offers a comprehensive set of powerful, flexible, and simple-to-use services that enable you to extract insights and actionable information in real time. Amazon Kinesis is a platform for streaming data on AWS, offering key capabilities to cost-effectively process streaming data at any scale. Kinesis capabilities include Amazon Kinesis Data Analytics, the easiest way to process streaming data in real time with standard SQL without having to learn new programming languages or processing frameworks.

* (Incorrect) AWS Lambda

* (Incorrect) AWS RedShift

* (Selected) AWS Kinesis Streams

Incorrect. Kinesis Streams can stream data real-time, but Kinesis Data Analytics is designed to meet this requirement.

---
## Domain 4: Design Cost-Optimized Architectures

**Question 49**

After an IT Steering Committee meeting, you have been put in charge of configuring a hybrid environment for the company’s compute resources. You weigh the pros and cons of various technologies based on the requirements you are given. The main requirements to drive this selection are overall cost considerations and the ability to reuse existing internet connections. Which technology best meets these requirements?

* (Incorrect) AWS Direct Gateway

* (Correct) AWS Managed VPN

AWS Managed VPN lets you reuse existing VPN equipment and processes, and reuse existing internet connections.

It is an AWS-managed high availability VPN service.

It supports static routes or dynamic Border Gateway Protocol (BGP) peering and routing policies.

https://docs.aws.amazon.com/whitepapers/latest/aws-vpc-connectivity-options/network-to-amazon-vpc-connectivity-options.html

* (Selected) VPC Peering

Incorrect: A VPC peering connection is a networking connection between two VPCs that enables you to route traffic between them using private IPv4 addresses or IPv6 addresses. https://docs.aws.amazon.com/vpc/latest/peering/what-is-vpc-peering.html

* (Incorrect) AWS Direct Connect