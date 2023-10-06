# Chapter 19. Serverless Overviews from a Solution Architect Perspective

---
## Best Practices for AWS Lambda

* By default, Lambda operates in a public AWS-owned VPC, that can reach any public internet address. You should only enable your VPC for Lambda if you need to interact with a private resource located in a private subnet, such as RDS.

* Deploy common code to a Lambda layer, e.g. logging etc.

* The bigger your deployment package, the slower your function will cold-start. Remove all unnecessary items, including documentation and unused libraries.

* Monitor your concurrencies by setting a CloudWatch alarm to notify your team when the number of invocations exceed your threshold. You should also set up an AWS Budget to monitor costs on a daily basis.

* Over-provision your memory in order to run your functions faster and potentially reduce costs. But do not over-provision your function time out settings as it will lead to longer than expected running costs.

---
## Reference

* [Best Practices for Developing on AWS Lambda](https://aws.amazon.com/blogs/architecture/best-practices-for-developing-on-aws-lambda/)