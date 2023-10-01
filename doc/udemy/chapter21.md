# Chapter 21. Databases in AWS

---
## DynamoDB

### Point-In-Time Recovery

You can create either on-demand backups or enable continuous backups using point-in-time recovery of your Amazon DynamoDB tables. With point-in-time recovery, you don't have to worry about creating, maintaining, or scheduling on-demand backups.

For example, suppose that a test script writes accidentally to a production DynamoDB table. With point-in-time recovery, you can restore that table to any point in time during the last 35 days.

In addition, point-in-time operations don't affect performance or API latencies of your DynamoDB.

---
## References

* [Point-in-time recovery for DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/PointInTimeRecovery.html)