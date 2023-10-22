<!-- TOC -->

- [Chapter 17. Decoupling Applications: SQS, SNS, Kinesis, Active MQ](#chapter-17-decoupling-applications-sqs-sns-kinesis-active-mq)
    - [Amazon SQS](#amazon-sqs)
        - [SQS Queue URL](#sqs-queue-url)
        - [Metadata](#metadata)
    - [Dead-Letter Queue](#dead-letter-queue)
    - [Delay Queue vs Visibility Timeout](#delay-queue-vs-visibility-timeout)
    - [Long Polling vs Short Empty Polling](#long-polling-vs-short-empty-polling)
    - [Temporary Queues](#temporary-queues)
    - [Moving From a Standard Queue to a FIFO Queue](#moving-from-a-standard-queue-to-a-fifo-queue)
    - [FIFO Queues with Multiple Groups](#fifo-queues-with-multiple-groups)
    - [Amazon Kinesis](#amazon-kinesis)
    - [AWS Kinesis Best Practices](#aws-kinesis-best-practices)
    - [References](#references)

<!-- /TOC -->

# Chapter 17. Decoupling Applications: SQS, SNS, Kinesis, Active MQ

---
## Amazon SQS

SQS is a fully managed queuing service that helps decouple applications and microservices to increase fault tolerance. SQS queues come in two distinct types:

* Standard SQS queues are able to scale to enormous throughput with at-least-once delivery.

* FIFO queues are designed to guarantee that messages are processed exactly once in the exact order that they are received and have a default rate of 300 transactions per second (and up to 3,000 tps with batching).

### SQS Queue URL

* SQS assigns each queue you create a queue URL based on its queue name and other SQS compoenents, e.g. `sqs.us-east-2.amazonaws.com/123456789012/MyStandardQueue`.

* The name of a FIFO queue must end with `.fifo`, and this suffix counts toward the 80-character queue name limit.

* For FIFO queues, there are additional identifiers, such as message deduplication ID, message group ID and sequence number.

### Metadata

SQS lets you include metadata, such as timestamps, geospatial data etc, with messages using message attributes. Each message can have up to 10 message attributes, and each consists of:

* Name - message attribute name.
* Type - supported types include String, Number, Binary, and Custom.
* Value - the message attribute value.

---
## Dead-Letter Queue

A dead-letter queue (DLQ) lets other queues sent their messages that cannot be processed in order for you to determine why their processing didnt succeed and to prevent the other queues from being blocked by an unconsumed message.

* The `maxReceiveCount` is the number of times a consumer tries receiving a message from a queue without deleting it before being moved to the DLQ.

* The redrive allow policy specifies which source queues can access the DLQ.

* Don't use a DLQ with FIFO queue if you don't want to break the exact order of messages.

---
## Delay Queue vs Visibility Timeout

Delay queues let you postpone delivery of new messages by hiding them up to a maximum of 15 minutes. Delay queues are similar to visibility timeouts because both features make messages unavailable to consumers for a specific period of time.

The difference between the two is that, for delay queues, a message is hidden when it is first added to queue, whereas for visibility timeouts a message is hidden only after it is consumed from the queue.

---
## Long Polling vs Short (Empty) Polling

While the regular short polling returns immediately even if the message queue being polled is empty, long polling doesn't return a response until a message arrives in the message queue, or the long poll times out.

Long polling are billed exactly the same as short polling. However, using long polling might reduce the cost of using SQS, because you can reduce the number of empty receives.

---
## Temporary Queues

When using a common request-response message pattern, you can use a temporary queue to create a high-throughput and cost-effective queue. For example, many applications use queues to communicate between frontends and backends when processing a login request from a user.

* Creating a queue is often faster and easier than creating an HTTPS endpoint.

* Queues are safe by default because they are locked down to the AWS account that created them.

* Any DDoS attempt on your service is absorbed by the SQS instead of loading down your own servers.

* There is no need to create firewall rules for communication between microservices if they use queues.

* However, creating a new queue for each response is like building a road just so a single car can drive on it before tearing it down.

To better support short-lived messaging, the SQS Temporary Queue Client makes it easy to create and delete many temporary queues. The key concept behind the client is the virtual queue.

Virtual queues let you multiplex many low-traffic queues onto a single SQS queue. As there is no API call to SQS, there is no costs associated with creating a virtual queue.

A single call to SQS can provide messages for up to 10 virtual queues, reducing the API calls by up to a factor of ten. Virtual queues are implemented entirely within the Temporary Queue Client, but not SQS itself.

---
## Moving From a Standard Queue to a FIFO Queue

* You cannot convert an existing standard queue to a FIFO queue. You must delete your existing standard queue and recreate it as a FIFO queue.

* FIFO queues don't support per-message delay, only per-queue delay. If your application sets the value of `DelaySeconds` parameter on each message, you must modify your application to set `DelaySeconds` on the entire queue instead.

* Message group is a unique FIFO feature that enables customers to process messages in parallel while maintaining their respective ordering.

* Modify your application to provide a unique message deduplication ID for each sent message, if your application can send identical message bodies.

* Otherwise, enable content-based deduplication, if your application sends messages with unique message bodies.

---
## FIFO Queues with Multiple Groups

Suppose you run a major auctions platform where people buy and sell a wide range of products. Your platform requires that transactions from buyers and sellers get processed in exactly the order received.

All transactions for Auction A need to be kept in order and so do all transactions for Auction B. But the two auctions are independent and it does not matter which auctions transactions aare processed first.

FIFO can handle that case with a feature called message groups. Each transaction related to Auction A is placed by your producer into a message group A, and so on. The FIFO queue always keeps transactions within a message group in the order in which they arrived.

The consumer now gets the messages ordered by message groups, all the B group messages followed by all the A group messages. If FIFO cannot fill up a batch of messages with a single message group, FIFO can place more than one message group in a batch of messages. But whenever possible, the queue gives you a full batch of messages from the same group.

The order of messages leaving a FIFO queue is governed by three rules:

1. Return the oldest message where no other message in the same message group is currently in-flight.

2. Return as many messages from the same message group as possible.

3. If a message batch is still not full, go back to rule 1.

---
## Amazon Kinesis

Amazon Kinesis makes it easy to collect, process, and analyze streaming data in real-time, such as application logs, IoT telemetry data etc.

There are four types of Kinesis:

* Kinesis Data Streams - capture, process and store data.

* Kinesis Data Firehose - load data into AWS data lakes, data stores and analytic services, such as S3, Redshift, OpenSearch, and service providers like Datadog, Splunk etc.

* Kinesis Data Analytics - transform and analyze data with SQL or Apache Flink.

* Kinesis Video Streams - capture, process and store video.

---
## AWS Kinesis Best Practices

A common use case is to consolidate and enrich logs from applications in real-time to proactively identify and resolve failure scenarios to significantly reduce application downtime.

To help ingest real-time data, you can use Amazon Kinesis Data Streams, which can continuously capture GBs of data per second from hundreds of thousands of sources. You can then use the following resource to process records in a Kinesis data stream:

* AWS Lambda - used for processing individual shards, focusing on business logic, without worrying about polling, checkpointing and error handling complexities. For example, filtering out non-critical logs from an application.

* AWS Kinesis Data Analytics - used for correlation of events of different shards, stateful stream processing, such as windowed aggregations.

* AWS Kinesis Data Firehose - used for buffering large volumes of streaming data before writing elsewhere.

---
## References

* [Solving Complex Ordering Challenges with Amazon SQS FIFO Queues](https://aws.amazon.com/blogs/compute/solving-complex-ordering-challenges-with-amazon-sqs-fifo-queues/)

* [Best practices for consuming Amazon Kinesis Data Streams using AWS Lambda](https://aws.amazon.com/blogs/big-data/best-practices-for-consuming-amazon-kinesis-data-streams-using-aws-lambda/)

* [Moving from a standard queue to a FIFO queue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/FIFO-queues-moving.html)

* [Amazon SQS features and capabilities](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/features-capabilities.html)

* [Simple Two-way Messaging using the Amazon SQS Temporary Queue Client](https://aws.amazon.com/blogs/compute/simple-two-way-messaging-using-the-amazon-sqs-temporary-queue-client)