# Chapter 13. Decoupling Workflows

<!-- TOC -->

- [Chapter 13. Decoupling Workflows](#chapter-13-decoupling-workflows)
  - [Decoupling Workflows Overview](#decoupling-workflows-overview)
    - [Tight vs Loose Coupling](#tight-vs-loose-coupling)
  - [Poll-Based Messaging with SQS](#poll-based-messaging-with-sqs)
    - [SQS Settings](#sqs-settings)
    - [Exam Tips](#exam-tips)
  - [Sidelining Messages with Dead-Letter Queues](#sidelining-messages-with-dead-letter-queues)
  - [Ordered Messages with SQS FIFO](#ordered-messages-with-sqs-fifo)
  - [Delivering Messages with SNS](#delivering-messages-with-sns)
  - [Fronting Applications with API Gateway](#fronting-applications-with-api-gateway)
  - [Executing Batch Workloads Using AWS Batch](#executing-batch-workloads-using-aws-batch)
  - [Brokering Messages with Amazon MQ](#brokering-messages-with-amazon-mq)
  - [Coordinating Distributed Apps with AWS Step Functions](#coordinating-distributed-apps-with-aws-step-functions)
  - [Ingesting Data from SaaS Applications to AWS with Amazon AppFlow](#ingesting-data-from-saas-applications-to-aws-with-amazon-appflow)
  - [Exam Tips](#exam-tips)

<!-- /TOC -->

---
## Decoupling Workflows Overview

### Tight vs Loose Coupling

Tight coupling means that one instance (FE) talks directly to another instance (BE), while loose coupling means that the FE talks indirectly to the BE via ELB.

However, an ELB requires the BE instances to be active and online all the time. Alternative AWS services can be used to replace the ELB, when our BE instances are not active at all times.

* Simple Queue Service (SQS)

* Simple Notification Service (SNS)

* API Gateway

---
## Poll-Based Messaging with SQS

SQS is a messaging queue that **allows asynchronous processing of work**, where one resource will write a message to an SQS queue, and then another resource will retrieve that message from SQS.

### SQS Settings

* Delivery Delay - Default is zero, but can be set up to 15 minutes.

* Message Size - **Can send up to 256 KB of text** in any format.

* Encryption - Default is enabled to be encrypted in transit by default, while at rest encrpytion requires a KMS key.

* Message Retention - Default is 4 days, but can be set up to 14 days.

* Polling Type - [**Long**|**Short**] Long polling isn't the default, but it should be, which means a long waiting time before polling the SQS queue for messages.

* Queue Depth - This can be a trigger for autoscaling.

* Visibility Timeout - Ensure proper handling of messages by the receiver, where a message remains in the queue while SQS waits for a response from the first receiver by locking it for 30 seconds, but no other receivers can view it. The message is deleted after the visibility timeout has expired.

### Exam Tips

---
## Sidelining Messages with Dead-Letter Queues

---
## Ordered Messages with SQS FIFO

---
## Delivering Messages with SNS

---
## Fronting Applications with API Gateway

---
## Executing Batch Workloads Using AWS Batch

---
## Brokering Messages with Amazon MQ

---
## Coordinating Distributed Apps with AWS Step Functions

---
## Ingesting Data from SaaS Applications to AWS with Amazon AppFlow

---
## Exam Tips
