<!-- TOC -->

- [Amazon SQS](#amazon-sqs)
- [FIFO Queues with Multiple Groups](#fifo-queues-with-multiple-groups)
- [References](#references)

<!-- /TOC -->

---
## Amazon SQS

SQS is a fully managed queuing service that helps decouple applications and microservices to increase fault tolerance. SQS queues come in two distinct types:

* Standard SQS queues are able to scale to enormous throughput with at-least-once delivery.

* FIFO queues are designed to guarantee that messages are processed exactly once in the exact order that they are received and have a default rate of 300 transactions per second.


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