<!-- TOC -->

- [Domain 2: Design Resilient Architectures](#domain-2-design-resilient-architectures)
- [Domain 4: Design High-Performing Architectures](#domain-4-design-high-performing-architectures)

<!-- /TOC -->

**Question 10**

* Service Control Policies (SCP)

Affected by SCPs:
1. all users and roles in the member accounts
2. root user of the member accounts
3. IAM permission policies in the member accounts

Not affected by SCPs:
1. service-linked roles

**Question 18**

* Allowed custom source for an inbound rule for a security group.

Allowed:
1. range of IP addresses in CIDR block notation
2. security group
3. IP address

Not allowed:
1. Internet Gateway ID

**Question 33**

* CloudFront access to static content in S3 bucket should only be allowed from a limited set of IP ranges.

Associate with CloudFront:
1. WAF
2. Origin Access Identity (OAI)

Cannot associate with CloudFront:
1. NACL
2. Security group

Cannot associate with S3 bucket:
1. WAF

**Question 41**

* Resolve DNS queries in the on-premises network from AWS VPC.
* Resolve DNS queries in the AWS VPC from on-premises network.

Create in Route 53 Resolver:
1. Inbound endpoint to receive DNS queries from on-premises network
2. Outbound endpoint to forward DNS queries to on-premises network

Cannot create in Route 53 Resolver:
1. Universal endpoint

**Question 51**

* AWS Direct Connect connection from on-premises data center to the AWS VPC.
* Site-to-Site VPN connections to connect from branch offices to the AWS VPC.
* branch offices to send and receive data with each other and their data center.

AWS to offsite:
1. VPN CloudHub
2. Software VPN

AWS to AWS:
1. VPC Endpoint
2. VPC Peering

Offsite to offsite:
1. VPN CloudHub

**Question 53**

* Sharing AMIs across different AWS accounts in multiple AWS regions.

Share AMIs:
* different AWS accounts
* multiple AWS regions
* change status from unencrypted to encrypted (but not vice-versa)
* both EBS-backed and instance-store-backed AMIs
* shared AMIs from other AWS accounts
* distinct target AMI ID
* distinct and full target AMI snapshot
* copied user-defined AMI tags (but not AWS tags)
* no charges (except for storage and data transfer)

---
## Domain 2: Design Resilient Architectures

**Question 2**

* Cloud file storage that provide full Windows capability.
* Standard SMB protocol compatible with Windows systems.

Supports SMB and Windows:
1. File Gateway of Storage Gateway
2. FSx for Windows File Server

Does not support SMB:
1. S3
2. EFS
3. EBS

**Question 13**

* Decouple architecture and adopt microservices.
* Connect microservices that consist of both fast and slow running processes.

Tightly coupled:
1. EventBridge

Decoupled pub-sub push:
1. SNS

Decoupled pub-sub poll:
1. SQS

Decoupled pub-sub parallel (ordered):
1. Kinesis Data Streams

**Question 26**

* Recover EC2 instances automatically using CloudWatch alarms.

Retain in recovered instances:
* identical to the original instance, including instance ID, private IPs, Elastic IPs and metadata.
* retains the public IPs
* impaired instances
* EBS volumes

Not retain in recovered instances:
* in-memory data
* terminated or stopped instances
* instance store volumes

---
## Domain 4: Design High-Performing Architectures

**Question 24**

* Most frequently accessed logs are available as cached data locally.
* Backing up all logs on S3 bucket.

Frequently accessed logs cached locally:
1. AWS Volume Gateway - Cached Volume

Non-frequently access logs cached locally:
1. AWS Volume Gateway - Stored Volume

Non-cached:
1. Snowball Edge Storage Optimized
2. Direct Connect

**Question 35**

* Move on-premises data into S3, EFS, and FSx for Windows File Server.
* Automate and accelerate online data transfers.

Online data transfer to S3, EFS, FSx for Windows, CloudWatch, CloudTrail:
1. DataSync

Online data transfer to S3 and EFS only:
1. Transfer Family

Online data transfer to S3 only:
1. File Gateway

Offline data transfer:
1. Snowball Edge Storage Optimized

**Question 40**

* Uses Microsoft Active Directory (AD) to provide users with access to resources on the on-premises infrastructure.
* Run directory-aware workloads on AWS in the form of a hybrid cloud.
* Configure a trust relationship to enable SSO for its users.

Run directory-aware workloads and configure trust relationship:
1. AWS Managed Microsoft AD

Cannot run directory-aware workloads:
1. AD Connector
2. Cloud Directory

Run directory-aware workloads but cannot configure trust relationship:
1. Simple AD
