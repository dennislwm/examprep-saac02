# Chapter 21. Databases in AWS

<!-- TOC -->

- [Chapter 21. Databases in AWS](#chapter-21-databases-in-aws)
    - [DynamoDB](#dynamodb)
        - [Point-In-Time Recovery](#point-in-time-recovery)
    - [References](#references)

<!-- /TOC -->

---
## DynamoDB

In June 2017, DynamoDB released auto scaling to make it easier for you to manage capacity efficiently. DynamoDB auto scaling uses a scaling policy, where you set the minimum and maximum levels of read and write capacity in addition to the target utilization percentage.

Auto scaling uses CloudWatch to monitor a table's read and write metrics. To do so, it creates CloudWatch alarms that track consumed capacity. The alarms are triggered when the upper or lower thresholds are breached for a period of time.

### Auto scaling: Provisioned, On-Demand, and Reserved Capacities

Provisioned capacity is the default model, where you specify the number of reads and writes per second that you require for your application. You can use auto scaling to adjust your table's provisioned capacity automatically in response to traffic changes.

Provisioned mode is a good option if any of the following are true:

* predictable application traffic.
* traffic that ramps up or down gradually.
* can forecast capacity requirements to control costs.

With On-Demand, DynamoDB instantly allocates capacity as it is needed. There is no delay and On-Demand is ideal for bursty, new or unpredictable workloads.

On-Demand mode is a good option if any of the following are true:

* create new tables with unknown workloads.
* unpredictable application traffic.
* prefer the ease of paying for only what you use.

Reserved capacity for DynamoDB is consistent with EC2 Reserved Instance model. You purchase reserved capacity for a one-year or three-year term and receive a significant discount.

### Point-In-Time Recovery

You can create either on-demand backups or enable continuous backups using point-in-time recovery of your Amazon DynamoDB tables. With point-in-time recovery, you don't have to worry about creating, maintaining, or scheduling on-demand backups.

For example, suppose that a test script writes accidentally to a production DynamoDB table. With point-in-time recovery, you can restore that table to any point in time during the last 35 days.

In addition, point-in-time operations don't affect performance or API latencies of your DynamoDB.

---
## References

* [Point-in-time recovery for DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/PointInTimeRecovery.html)

* [Amazon DynamoDB auto scaling: Performance and cost optimization at any scale](https://aws.amazon.com/blogs/database/amazon-dynamodb-auto-scaling-performance-and-cost-optimization-at-any-scale/)

* [Read/write capacity mode](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ReadWriteCapacityMode.html)