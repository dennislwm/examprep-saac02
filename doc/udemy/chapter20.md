# Chapter 20. Serverless Solution Architecture

---
## Mobile Application

* Expose a REST API with HTTPS.
* Serverless architecture.
* Users should be able to directly interact with their own folder in S3.
* Users should authenticate through a managed serverless service.
* Users can write and read, but they mostly read.
* The database should scale, and have some high read throughput.

1. Use an API Gateway to expose REST API with HTTPS endpoints.
2. The API Gateway invokes multiple Lambda functions that reads and writes to DynamoDB.
3. The API Gateway uses AWS Cognito to authenticate requests.
4. AWS Cognito can generate temporary credentials using STS.
5. Users can interact directly with their own folder in S3 using the temporary credentials.
6. For high read throughput, and static data, we can add DAX caching layer.
7. We can further improve throughput by adding cache at the API Gateway.

---
## Web Application

* Web site should scale globally.
* Users can write and read, but they mostly read.
* Caching must be implemented where possible.
* Any new users that subscribes should receive a welcome email.
* Any photo uploaded should have a thumbnail generated.
* Web site contains both static and dynamic content.

1. Use an S3 bucket for hosting static content.
2. Use CloudFront with origin as S3 bucket.
3. Only authorize CloudFront in our bucket policy.
4. Use an API Gateway to expose REST API for dynamic content.
5. The API Gateway invokes multiple Lambda functions for reads and writes to DynamoDB Global Tables.
6. For caching, we can enable API Gateway caching, and also add a DAX caching layer in front of our DynamoDB Global Tables.
7. For new subscribers, we can enable DynamoDB Stream to detect changes to a table, which in turn invokes a Lambda function.
8. The new subscriber Lambda function should have an IAM role that allows sending mail via SES.
9. For thumbnails, we can enable S3 Transfer Acceleration so that uploads images through CloudFront into an S3 bucket.
10. The images S3 bucket triggers a Lambda function that generates a thumbnail for each image upload and stores it into another S3 bucket.

---
## Microservice Architecture

* Many services interact with each other directly using a REST API.
* Each server scales independently and has its own code repository.

First microservice:
1. Use an ELB to accept HTTP requests from users.
2. The ELB is in front of an ECS that manages docker containers that reads and writes to DynamoDB.
3. Use Route 53 for managing domain name of microservice.

Second microservice:
1. Use an API Gateway to expose REST API for microservice.
2. The API Gateway invokes a Lambda function for microservice that reads and writes to ElastiCache Redis.
3. The Lambda function may interact with the ELB of the first microservice.

Third microservice:
1. Use an ELB to accept HTTP requests from users.
2. Use an EC2 Autoscaling group to manage instances that reads and writes to RDS.
3. Use Route 53 for managing domain name of microservice.
4. The EC2 instances may interact with the API Gateway of the second microservice.

---
## Software Updates Offloading

* Application running on EC2 that distributes software updates.
* Optimize cost of distribution.

1. ELB + ASG with EC2 instances.
2. EFS volumes to store software updates.
3. CloudFront with origin to ELB.
4. No changes required to architecture, as CloudFront will scale out on demand.
5. Software updates (static files) are cached at the edge location.
6. CloudFront costs is cheaper than ASG scaling.

---
## E-Commerce Online Transaction Processing

* Event-driven architecture with decoupled microservices that communicate with each other.
* An order service emits an OrderCreated event when a new order is created.
* The shipping service then consumes this event that starts the shipping process.
* The transactional nature ensures that all related events are persisted or none of them are.
* A transactional layer consists of an upstream Lambda function and two DynamoDBs.
* A transactional layer handles the complexity of the events schema.
* A downstream Lambda function should be decoupled from the events schema.
* A downstream Lambda function should only need to relay events.

1. A Lambda function to accept OrderCreated event and writes to two DynamoDB tables.
2. The OrdersTable contains the order and will persists only if all events persist.
3. The OutboxTable is configured with a DynamoDB stream, such that every change to it triggers a downstream Lambda.
4. A downstream Lambda function to accept PublishEvent events and relay them to an EventBridge.
5. The downstream consumer of the events should be idempotent (able to handle the same event multiple times).

---
## Reference

* [Don't Lose Your Events! Use the Transactional Outbox Pattern](https://dev.to/aws-builders/dont-lose-your-events-use-the-transactional-outbox-pattern-ggo)