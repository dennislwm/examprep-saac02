# More Solution Architectures

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