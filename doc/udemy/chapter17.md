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
## References

* [Solving Complex Ordering Challenges with Amazon SQS FIFO Queues](https://aws.amazon.com/blogs/compute/solving-complex-ordering-challenges-with-amazon-sqs-fifo-queues/)