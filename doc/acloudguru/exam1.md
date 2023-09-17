# Practice Exam 1

---
## Domain 1: Design Secure Architectures

**Question 7**

You are managing S3 buckets in your organization. This management of S3 extends to Amazon Glacier. For auditing purposes you would like to be informed if an object is restored to S3 from Glacier. What is the most efficient way you can do this?

* (Correct) Configure S3 notifications for restore operations from Glacier

The Amazon S3 notification feature enables you to receive notifications when certain events happen in your bucket. To enable notifications, you must first add a notification configuration that identifies the events you want Amazon S3 to publish and the destinations where you want Amazon S3 to send the notifications. An S3 notification can be set up to notify you when objects are restored from Glacier to S3.

* (Incorrect) Create a Lambda function which is triggered by restoration of object from Glacier to S3

* (Incorrect) Create an SNS notification for any upload to S3

This would involve a CloudWatch Event, which could then use SNS for notification. But this would be triggered by any upload event. https://docs.aws.amazon.com/AmazonS3/latest/dev/NotificationHowTo.html#notification-how-to-event-types-and-destinations

* (Selected) Create a CloudWatch Event for uploads to S3

**Question 35**

A small startup company has multiple departments with small teams representing each department. They have hired you to configure Identity and Access Management in their AWS account. The team expects to grow rapidly, and promote from within which could mean promoted team members switching over to a new team fairly often. How can you configure IAM to prepare for this type of growth?

* (Selected) Create the user accounts, create a group for each department, create and attach an appropriate role to each group, and place each user account into their department’s group. When new team members are onboarded, create their account and put them in the appropriate group. If an existing team member changes departments, move their account to their new IAM group.

Incorrect. You attach a policy, not a role, to a group to give those users in the group the appropriate permissions.

* (Incorrect) Create the user accounts, create a group for each department, create and attach an appropriate policy to each group, and place each user account into their department’s group. When new team members are onboarded, create their account and put them in the appropriate group. If an existing team member changes departments, delete their account, create a new account and put the account in the appropriate group.

* (Incorrect) Create the user accounts, create a role for each department, create and attach an appropriate policy to each role, and place each user account into their department’s role. When new team members are onboarded, create their account and put them in the appropriate role. If an existing team member changes departments, move their account to their new IAM group.

* (Correct) Create the user accounts, create a group for each department, create and attach an appropriate policy to each group, and place each user account into their department’s group. When new team members are onboarded, create their account and put them in the appropriate group. If an existing team member changes departments, move their account to their new IAM group.

An IAM group is a collection of IAM users. Groups let you specify permissions for multiple users, which can make it easier to manage the permissions for those users. For example, you could have a group called Admins and give that group the types of permissions that administrators typically need. Any user in that group automatically has the permissions that are assigned to the group. If a new user joins your organization and needs administrator privileges, you can assign the appropriate permissions by adding the user to that group. Similarly, if a person changes jobs in your organization, instead of editing that user's permissions, you can remove the user from the old groups and add the user to the appropriate new groups.

**Question 47**

You have been put in charge of S3 buckets for your company. The buckets are separated based on the type of data they are holding and the level of security required for that data. You have several buckets that have data you want to safeguard from accidental deletion. Which configuration will meet this requirement?

* (Correct) Enable versioning on the bucket and multi-factor authentication delete as well.

Correct. Versioning is a means of keeping multiple variants of an object in the same bucket. You can use versioning to preserve, retrieve, and restore every version of every object stored in your Amazon S3 bucket. With versioning, you can easily recover from both unintended user actions and application failures. When you enable versioning for a bucket, if Amazon S3 receives multiple write requests for the same object simultaneously, it stores all of the objects. Key point: versioning is turned off by default. If a bucket's versioning configuration is MFA Delete–enabled, the bucket owner must include the x-amz-mfa request header in requests to permanently delete an object version or change the versioning state of the bucket. References: https://docs.aws.amazon.com/AmazonS3/latest/dev/Versioning.html https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingMFADelete.html

* (Incorrect) Archive sensitive data to Amazon Glacier.

* (Incorrect) Signed URLs to all users to access the bucket.

* (Selected) Configure cross-account access with an IAM Role prohibiting object deletion in the bucket.

Incorrect. Cross-account access has to do with the ability to access buckets in other accounts. A second account was not mentioned in the question. And you do not want to completely prohibit deletion in the bucket. There may be valid reasons to delete objects. The key phrase is “safeguard from accidental deletion”.

**Question 52**

Your company has gotten back results from an audit. One of the mandates from the audit is that your application, which is hosted on EC2, must encrypt the data before writing this data to storage. It has been directed internally that you must have the ability to manage dedicated hardware security module instances to generate and store your encryption keys. Which service could you use to meet this requirement?

* (Incorrect) Amazon EBS encryption

* (Incorrect) AWS Security Token Service

* (Selected) AWS KMS

If you need to secure your encryption keys in a service backed by FIPS-validated HSMs, but you do not need to manage the HSM, you can try AWS Key Management Service.

* (Correct) AWS CloudHSM

The AWS CloudHSM service helps you meet corporate, contractual, and regulatory compliance requirements for data security by using dedicated Hardware Security Module (HSM) instances within the AWS cloud. A Hardware Security Module (HSM) provides secure key storage and cryptographic operations within a tamper-resistant hardware device. HSMs are designed to securely store cryptographic key material and use the key material without exposing it outside the cryptographic boundary of the hardware. You should use AWS CloudHSM when you need to manage the HSMs that generate and store your encryption keys. In AWS CloudHSM, you create and manage HSMs, including creating users and setting their permissions. You also create the symmetric keys and asymmetric key pairs that the HSM stores. AWS Documentation: When to use AWS CloudHSM.

---
## Domain 2: Design Resilient Architectures

**Question 14**

Your company has performed a Disaster Recovery drill which failed to meet the Recovery Time Objective (RTO) desired by executive management. The failure was due in large part to the amount of time taken to restore proper functioning on the database side. You have given management a recommendation of implementing synchronous data replication for the RDS database to help meet the RTO. Which of these options can perform synchronous data replication in RDS?

* (Incorrect) DAX

* (Selected) AWS Database Migration Service

The Database Migration Service, as the name implies, helps to migrate databases into AWS.

* (Correct) RDS Multi-AZ

Amazon RDS Multi-AZ deployments provide enhanced availability and durability for RDS database (DB) instances, making them a natural fit for production database workloads. When you provision a Multi-AZ DB instance, Amazon RDS automatically creates a primary DB instance and synchronously replicates the data to a standby instance in a different Availability Zone (AZ). Each AZ runs on its own physically distinct, independent infrastructure, and is engineered to be highly reliable. In case of an infrastructure failure, Amazon RDS performs an automatic failover to the standby (or to a read replica in the case of Amazon Aurora), so that you can resume database operations as soon as the failover is complete. Since the endpoint for your DB instance remains the same after a failover, your application can resume database operation without the need for manual administrative intervention. https://aws.amazon.com/rds/features/multi-az/

* (Incorrect) Read replicas

**Question 30**

A financial tech company has decided to begin migrating their applications to the AWS cloud. Currently, they host their entire application using several self-managed Kubernetes clusters. One of their major concerns during this migration is monitoring and collecting system metrics due to the very large-scale deployments that are in place. Your Chief Technology Officer wants to avoid using AWS-proprietary technology-based monitoring services and instead leverage existing, well-known, open-source applications to help meet the monitoring requirements. Which combination of the following AWS services would best fit the company requirements while minimizing operational overhead?

* (Correct) AWS Managed Service for Prometheus

Prometheus offers open-source monitoring. Amazon Managed Service for Prometheus is a serverless, Prometheus-compatible monitoring service for container metrics. It is perfect for monitoring Kubernetes clusters at scale. Reference: What is Amazon Managed Prometheus

* (Incorrect) AWS Config

* (Selected) Grafana on Auto Scaling EC2 Instances

While Grafana is a popular open-source analytics and monitoring application, running it on EC2 instances immediately adds operational overhead and cost to the solution.

* (Selected) Prometheus on Auto Scaling EC2 Instances

While Prometheus is a popular open-source monitoring tool, running it on EC2 instances immediately adds operational overhead and cost to the solution.

* (Correct) AWS Managed Grafana

Grafana is a well-known open-source analytics and monitoring application. Amazon Managed Grafana offers a fully managed service for infrastructure for data visualizations. You can leverage this service to query, correlate, and visualize operational metrics from multiple sources. Reference: What is Amazon Managed Grafana

**Question 37**

An application team has decided to leverage AWS for their application infrastructure. The application performs proprietary, internal processes that other business applications utilize for their daily workloads. It is built with Apache Kafka to handle real-time streaming, which virtual machines running the application in docker containers consume the data from. The team wants to leverage services that provide less overhead but also cause the least amount of disruption to coding and deployments. Which combination of AWS services would best meet the requirements?

* (Incorrect) Amazon SNS

* (Incorrect) Amazon MQ

* (Correct) Amazon ECS Fargate

Fargate containers offer the least disruptive changes, while also minimizing the operational overhead of managing the compute services. Reference: What is AWS Fargate?

* (Correct) Amazon MSK

This service is meant for applications that currently use or are going to use Apache Kafka for messaging. It allows for managing of control plane operations in AWS. Reference: Welcome to the Amazon MSK Developer Guide

* (Selected) Amazon Kinesis Data Streams

This does not support Apache Kafka messaging protocols.

* (Incorrect) AWS Lambda

---
## Domain 3: Design High-Performing Architectures

**Question 36**

Your team has provisioned Auto Scaling groups in a single Region. The Auto Scaling groups, at max capacity, would total 40 EC2 On-Demand Instances between them. However, you notice that the Auto Scaling groups will only scale out to a portion of that number of instances at any one time. What could be the problem?

* (Correct) There is a vCPU-based On-Demand Instance limit per Region.

Your AWS account has default quotas, formerly referred to as limits, for each AWS service. Unless otherwise noted, each quota is Region specific. You can request increases for some quotas, and other quotas cannot be increased. Remember that each EC2 instance can have a variance of the number of vCPUs, depending on its type and your configuration, so it's always wise to calculate your vCPU needs to make sure you are not going to hit quotas easily. Service Quotas is an AWS service that helps you manage your quotas for over 100 AWS services from one location. Along with looking up the quota values, you can also request a quota increase from the Service Quotas console. Reference: AWS Service Quotas Reference: Amazon EC2 Endpoints and Quotas

* (Incorrect) You can have only 20 instances per Availability Zone.

* (Incorrect) You can have only 20 instances per Region. This is a hard limit.

* (Selected) The associated load balancer can serve only 20 instances at one time.

This is not true and is not a limiting factor.

**Question 62**

A team of architects is designing a new AWS environment for a company which wants to migrate to the Cloud. The architects are considering the use of EC2 instances with instance store volumes. The architects realize that the data on the instance store volumes are ephemeral. Which action will not cause the data to be deleted on an instance store volume?

* (Incorrect) The underlying disk drive fails.

Reboot

Correct. Some Amazon Elastic Compute Cloud (Amazon EC2) instance types come with a form of directly attached, block-device storage known as the instance store. The instance store is ideal for temporary storage, because data in the instance store is lost under any of the following circumstances:

* (Incorrect) The underlying disk drive fails

* (Incorrect) The instance stops

* (Incorrect) The instance terminates

* (Correct) Reboots

* (Incorrect) Hardware disk failure.

* (Selected) Instance is stopped

Incorrect. The data in an instance store persists only during the lifetime of its associated instance.

---
## Domain 4: Design Cost-Optimized Architectures

**Question 23**

Your team owns three separate AWS accounts: one for production, one for staging, and one for development. Recently, there has been a push from the CEO to begin breaking down costs to the most comprehensive, detailed level. In addition to this level of detail, the team needs to store daily comma-separated value (CSV) reports in Amazon S3 for ingestion into the company’s internal analytics tooling.

What would be the most efficient solution for this scenario?

* (Correct) Use AWS Cost and Usage Reports to generate reports, and have it export CSV reports daily to a centralized Amazon S3 bucket.

AWS Cost and Usage Reports offers the greatest amount of detail for spending reports. They can also be set up to automatically store updated reports in Amazon S3 every 24 hours.

* (Incorrect) Use AWS Budgets to alert and generate reports on current spend, and use AWS Fargate to pull data, generate CSV reports, and then push them to Amazon S3.

* (Selected) Use AWS Cost and Usage Reports to generate reports with the required amount of detail. Set up Amazon EventBridge (Amazon CloudWatch Events) to trigger a rule to create and then export CSV reports daily to a centralized Amazon S3 bucket.

While AWS Cost and Usage Reports offers the greatest amount of detail for spending reports, Amazon EventBridge (Amazon CloudWatch Events) does not convert or create reports for you to store in Amazon S3.

* (Incorrect) Use AWS Budgets to alert and generate reports, and use AWS Lambda to pull data, generate CSV reports, and then push them to Amazon S3.

**Question 25**

After an IT Steering Committee meeting, you have been put in charge of configuring a hybrid environment for the company’s compute resources. You weigh the pros and cons of various technologies based on the requirements you are given. The decision you make is to go with Direct Connect. Which option best describes the features Direct Connect provides?

* (Incorrect) A connection between on-premises and VPC, using secure and private connection with IPsec and TLS

* (Selected) A cost-effective, private network connection that bypasses the internet

Incorrect: This is not the best answer. The idea of Direct Connect being cost effective is probably highly-dependent on the situation. In most cases, Direct Connect would be considered fairly expensive and a judgement would be weighed on cost vs. need.

* (Correct) A private, dedicated network connection between your facilities and AWS

Correct: AWS Direct Connect is a cloud service solution that makes it easy to establish a dedicated network connection from your premises to AWS. Using AWS Direct Connect, you can establish private connectivity between AWS and your datacenter, office, or colocation environment, which in many cases can reduce your network costs, increase bandwidth throughput, and provide a more consistent network experience than internet-based connections.

AWS Direct Connect makes it easy to establish a dedicated connection from an on-premises network to one or more VPCs in the same region. Using private VIF on AWS Direct Connect, you can establish private connectivity between AWS and your data center, office, or colocation environment.

AWS Direct Connect can reduce network costs, increase bandwidth throughput, and provide a more consistent network experience than internet-based connections.

https://docs.aws.amazon.com/whitepapers/latest/aws-vpc-connectivity-options/aws-direct-connect.html

https://docs.aws.amazon.com/whitepapers/latest/aws-vpc-connectivity-options/network-to-amazon-vpc-connectivity-options.html

* (Incorrect) A network connection between two VPCs that can route traffic using IPv4 or IPv6