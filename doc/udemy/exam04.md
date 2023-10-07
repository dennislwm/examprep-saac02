# Practice Exam 4

---
## Domain 1: Design High-Performing Architectures

**Question 7**

* Designed the architecture of the website to be fully serverless with API Gateway and Lambda.
* The backend uses a DynamoDB table that stores all contracts.
* Least delay in sending automatic notifications to your developers on important milestones.

Can use DynamoDB to send notifications:
1. DynamoDB Streams
2. Amazon SQS

Cannot use DynamoDB to send notifications:
1. EventBridge
2. DynamoDB DAX

**Question 30**

* NLB directs traffic to 5 EC2 instances managed by an ASG.
* ASG scales out to 100 instances when a popular song is increased.
* Reduce costs without changing any of the application code.

Reduce bandwidth costs and changing application code:
1. S3

Reduce bandwidth costs no change of code:
1. CloudFront

Cannot reduce bandwith costs:
1. S3 Glacier
2. AWS Storage Gateway

**Question 48**

* Using AWS Site-to-Site VPN connections for secure connectivity to its AWS cloud resources from its on-premises data center.
* Users are experiencing slower VPN connectivity, how to maximize VPN throughput?

Can maximize VPN bandwidth:
1. Transit Gateway with equal costs multipath routing and add additional VPN tunnels.

Cannot maximize VPN bandwidth:
1. Transfer Acceleration
2. Virtual Private Gateway
3. Global Accelerator

---
## Domain 2: Design Resilient Architectures

**Question 2**

* Create a disaster recovery strategy leveraging AWS Cloud.
* Ensure that a scaled-down version of a fully functional environment is always running in the AWS cloud.
* Recovery time is kept to a minimum.

Non-scaled down, always running, minimal recovery time:
1. Multi Site

Scaled down, always running, minimal recovery time:
1. Warm Standby

Scaled down, always running, short recovery time:
1. Pilot Light

Non-scaled down, non-running, long recovery time:
1. Backup and Restore

**Question 5**

* Copied 1 PB of data from on-premises to S3 bucket in us-west-1.
* Set up a one-time copy of the data to another S3 in us-east-1.
* Does not allow the use of AWS Snowball.

One-time copy to S3 across region:
1. S3 batch replication
2. S3 sync

One-time copy to S3 in same region:
1. S3 Transfer Acceleration

Cannot copy to S3
1. S3 console

**Question 10**

* Company has a SaaS (Software as a Service) application that feeds updates to other in-house applications.
* The SaaS and in-house applications are being migrated to use AWS services for this inter-application communication.
* How would you asynchronously decouple the architecture?

Asynchronous decoupled resource with SaaS integration:
1. EventBridge

Asynchronous decoupled resource without SaaS integration:
1. SQS
2. SNS

Synchronous decoupled resource:
1. ELB

**Question 12**

* Whitelist up to two public IPs when accessing external services across the internet.
* Maintain HA, support scaling out to 10 instances.

HA, scalability with public IP:
1. NLB + ASG

HA, scalability without public IP:
1. ALB + ASG
2. Classic LB + ASG
3. ASG with Dynamic Elastic IP

**Question 23**

* Uses ElastiCache Redis to enhance the performance of its RDS database layer.
* Wants a robust DR strategy for its caching layer.
* Guarantees minimal downtime and data loss.

DR with minimal downtime and data loss for its caching layer:
1. Multi-AZ with automatic failover.

DR with high downtime and data loss for its caching layer:
1. Schedule daily automatic backups.
2. Schedule manual backups using Redis append-only file (AOF).

Non-DR:
1. Add read-replicas across multi AZs.

**Question 55**

* DR mechanism with minimum costs.
* Can only bear data loss of approximately 15 minutes.

Non-scaled down, always running, minimal recovery time:
1. Multi Site

Scaled down, always running, minimal recovery time:
1. Warm Standby

Scaled down, always running, short recovery time:
1. Pilot Light

Non-scaled down, non-running, long recovery time:
1. Backup and Restore

**Question 56**

* Build their APIs by using Docker containers.
* Facilitate hosting of containers by using orchestration services available with AWS.

Docker containers with orchestration:
1. EKS with Fargate
2. ECS with Fargate

Docker containers without orchestration:
1. ECS with EC2

Not Docker containers:
1. EMR
2. SageMaker

---
## Domain 3: Design Secure Architectures

**Question 29**

* A CRM web application was written as a monolith in PHP and is facing scaling issues.
* Re-engineer towards microservices architecture.
* Expose their application from the same LB, linked to different target groups with different URLS: `checkout.mycorp.com`, `www.mycorp.com`, `yourcorp.com/profile`, and `yourcorp.com/search`.
* Expose all these URLs as HTTPs endpoints for security purposes.
* Recommend a solution that requires minimal configuration effort.

TLS / SSL support for multiple domains with minimal effort:
1. Use SSL certificates with SNI.

TLS / SSL support for single domains with minimal effort:
1. Use a wildcard SSL certificate.
2. Use an HTTP to HTTPs redirect.
3. Change the ELB SSL Security Policy.


**Question 36**

* Setup 2-tier architecture consisting of an ALB and ASG managing a fleet of EC2 instances.
* The ALB is deployed in a subnet of size `10.0.1.0/24`.
* The ASG is deployed in a subnet of size `10.0.4.0/22`.
* Configure the security group of the EC2 instances to only allow traffic coming from the ALB.

Security group ingress rule from ALB:
1. Add a rule to authorize the security group of the ALB.

Security group ingress rule from ALB + CIDR:
1. Add rule to authorize the CIDR `10.0.1.0/24`.

Security group ingress rule not from ALB:
1. Add a rule to authorize the security group of the ASG.
2. Add a rule to authorize the CIDR `10.0.4.0/22`.

**Question 44**

* Designed the AWS Cloud architecture using API Gateway and AWS Lambda.
* The backend uses an RDS PostgreSQL database.
* The application uses a username and password combination to connect the Lambda function to the RDS database.
* Improve the security at the authentication level by leveraging short-lived credentials.

Improve the security:
1. Attach an AWS IAM role to AWS Lambda.
2. Use IAM Authentication from Lambda to RDS PostgreSQL.

Improve the security at the authentication level:
1. Embed a credential rotation logic in the AWS Lambda, retrieving them from SSM.
2. Restrict the RDS database security group to the Lambda's security group.
3. Deploy AWS Lambda in a VPC.

**Question 47**

* Deploy EC2 instances in a private subnet.
* Use VPC endpoints so that instances can access some AWS services.
* Two AWS services that support Gateway Endpoints.

Support VPC Gateway Endpoints:
1. S3
2. DynamoDB

Support VPC Interface Endpoints:
1. SNS
2. SQS
3. Kinesis

---
## Domain 4: Design Cost-Optimized Architectures

**Question 43**

* Photo processing company that has a proprietary algorithm to compress an image without any loss in quality.
* Clients are willing to wait for a response that carries their compressed images back.
* Process jobs asynchronously and scale quickly to cater to high demand.
* Job to be retried in case of failures.

Scalable queue and jobs retried in case of failures:
1. Spot Instances
2. SQS

Scalable and jobs cannot be interrupted:
1. On-Demand Instances
2. Reserved Instances

Scalable non-queue:
1. SNS

**Question 46**

* EBS storage volume (`io1`) accounts for 90% of the cost.
* Remaining 10% cost can be attributed to the EC2 instance.
* CloudWatch metric reports that both the EC2 instance and the EBS volume are under-utilized.
* EBS volume has occasional I/O bursts.
* The entire infrastructure is managed by AWS CloudFormation.
* How to reduce the cost.

SSD volumes optimized for frequent R/W with small I/O size and bursting IOPS:
1. Convert the EBS volume to `gp2`.

SSD volumes optimized for frequent R/W with small I/O size and provisioned IOPS:
1. Keep the EBS volume to `io1` and reduce the IOPS.

Not a volume:
1. Change the EC2 instance type to something much smaller.
2. Don't use a CloudFormation template to create the database as the CloudFormation service incurs greater service charges.

**Question 50**

* Heavy read traffic to RDS database.
* RDS database instance type is not cost-effective for their budget.
* Strategy to deal with the high volume of read traffic, reduce latency and downsize the instance size to cut costs.

Strategy to deal with high volume of read traffic, but not reduce latency or cut costs:
1. Setup RDS Read Replicas.
2. Move to Amazon Redshift.

Strategy to deal with high volume of read traffic, reduce latency and cut costs:
1. Setup ElastiCache in front of RDS.

Strategy that does not deal with high volume of read traffic, not reduce latency or cut costs:
1. Switch application code to AWS Lambda for better performance.

---
## Reference

* [Using Amazon Web Services for Disaster Recovery](https://d1.awsstatic.com/whitepapers/aws-disaster-recovery.pdf)