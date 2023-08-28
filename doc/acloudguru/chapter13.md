# Chapter 13. Decoupling Workflows

<!-- TOC -->

- [Chapter 13. Decoupling Workflows](#chapter-13-decoupling-workflows)
  - [Decoupling Workflows Overview](#decoupling-workflows-overview)
    - [Tight vs Loose Coupling](#tight-vs-loose-coupling)
  - [Poll-Based Messaging with SQS](#poll-based-messaging-with-sqs)
    - [SQS Settings](#sqs-settings)
  - [Sidelining Messages with Dead-Letter Queues](#sidelining-messages-with-dead-letter-queues)
    - [Dead-Letter Queues](#dead-letter-queues)
    - [Exam Tips](#exam-tips)
  - [Ordered Messages with SQS FIFO](#ordered-messages-with-sqs-fifo)
    - [Standard Order vs FIFO Order](#standard-order-vs-fifo-order)
  - [Push-Based Messaging with SNS Topics](#push-based-messaging-with-sns-topics)
    - [SNS Settings](#sns-settings)
  - [Fronting Applications with API Gateway](#fronting-applications-with-api-gateway)
    - [API Gateway Features](#api-gateway-features)
    - [Exam Tips](#exam-tips)
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

* FIFO or Standard Order - Default is Standard without ordering.

* Encryption - Default is enabled to be encrypted in transit by default, while at rest encrpytion requires a KMS key.

* Access Policy - A resource policy can be added to a queue, similar to S3.

* Message Retention - Default is 4 days, but can be set up to 14 days.

* Polling Type - [**Long**|**Short**] Long polling isn't the default, but it should be, which means a long waiting time before polling the SQS queue for messages.

* Queue Depth - This can be a trigger for autoscaling.

* Visibility Timeout - Ensure proper handling of messages by the receiver, where a message remains in the queue while SQS waits for a response from the first receiver by locking it for 30 seconds, but no other receivers can view it. If there is no response after the visibility timeout has expired, the message is unlocked, otherwise it is removed from the queue.

* Dead-letter Queue - Enable this if you want undeliverable messages to be sent to a custom SQS queue.

---
## Sidelining Messages with Dead-Letter Queues

Consider an edge scenario, where a message in the SQS queue has an invalid format, which wasn't validated by the frontend instance that sent it. A backend instance would poll for that message, and visibility timeout would lock it for 30 seconds, however there isn't any response from the backend as it failed to process the message.

Hence, after the visibility timeout had expired, the message would be unlocked and another backend instance would poll for that message, and a new visibility timeout would lock it for 30 seconds. This cycle would repeat itself for as long as the message retention period (up to a maximum of 14 days), where the message will be deleted forever.

### Dead-Letter Queues

A dead-letter queue is used to sidelined any invalid messages from the main SQS queue. The benefit is that the main queue is not blocked by any invalid messages, and this will allow subsequent valid messages to be processed by the backend.

In order to enable dead-letter queue:
1. You have to create a main queue and a secondary (dead-letter) queue.
2. In the main queue settings, enable the Dead-Letter Queue option, and select your secondary queue.
3. Under Dead-Letter queue, for **Maximum receives**, set a value between 1 and 1000.
  - This is the number of times the main queue will set the visibility timeout for a message, before sidelining it to the dead-letter queue.

### Exam Tips

* It is important to **set up a CloudWatch alarm to monitor queue depth**.

* You can set up SQS dead-letter queues for both SQS queues and SNS topics.

---
## Ordered Messages with SQS FIFO

### Standard Order vs FIFO Order

|               Standard               |                     FIFO                      |
|:------------------------------------:|:---------------------------------------------:|
|        At-least once delivery        |          First-in-first-out delivery          |
|         Best-effort ordering         |              Guaranteed ordering              |
|         Possible duplicates          |                 No duplicates                 |
| Near unlimited transactions per sec  |             300 messages per sec              |
| No message group or deduplication ID | Requires a message group and deduplication ID |
|              Lower cost              |                  Higher cost                  |

---
## Push-Based Messaging with SNS Topics

SNS is a push-based messaging that **proactively broadcast messages to multiple endpoints subscribed to it**, which can be used to alert a system or a person.

### SNS Settings

* Subscribers - message endpoints, e.g. Kinesis Data Firehose, email, Lambda, SQS etc.
  - Only SQS can use FIFO order.
  - Email requires an opt in from each recipient.
  - There is no retry for a failed message delivery, except for HTTP(S).

* Message Size - **Can send up to 256 KB of text** in any format.

* FIFO or Standard Order - FIFO only supports SQS as a subscriber.

* Encryption - Default is enabled to be encrypted in transit by default, while at rest encrpytion requires a KMS key.

* Access Policy - A resource policy can be added to a topic, similar to S3.

* Dead-letter Queue - Enable this if you want undeliverable messages to be sent to a custom SQS queue.

---
## Fronting Applications with API Gateway

AWS API Gateway is a fully managed service that **allows you to easily publish, create, maintain, monitor and secure your API**.

### API Gateway Features

* Security - Allows you to easily **protect your application endpoints by attaching a WAF**, where you can perform:
  - Rate-limiting
  - Blocking specific countries
  - Filtering malicious behavior
  - Implement DDoS protection

* Versioning - Supports versioning, using stages, of your API.

* Ease of use - Easily build out the calls that will trigger other AWS services in your account.

![Example API Gateway](../../img/acloudguru/Chp13.1.png)

### Exam Tips

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
