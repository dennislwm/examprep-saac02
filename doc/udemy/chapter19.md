# Chapter 19. Serverless Overviews from a Solution Architect Perspective

---
## Lambda Runtimes

You can use managed runtimes, or build your own. However since AWS cannot guarantee backward compatibility between major versions, this is a customer-driven operation.

For a Lambda function defined as a container image, you choose a runtime and the Linux distribution when you create the container image. To change the runtime, you create a new container image.

### Managed runtime deprecation policy

Deprecation (end of support) for a runtime occurs in two phases:

* Phase 1 - cannot create.
  - AWS no longer applies security patches or other updates to the runtime.
  - You cannot create functions that use the runtime, but you can continue to update existing functions.
  - Functions that use a deprecated runtime are not eligible for technical support.

* Phase 2 - cannot update or create.
  - You cannot update or create functions that use the runtime.
  - Phase 2 starts at least 30 days after the start of Phase 1.
  - Functions that use a deprecated runtime can still be invoked.

* Notification of Deprecation (end of support) for a runtime:
  - 120 days notice by AWS Trusted Advisor.
  - 60 days notice by email if you have functions using the runtime (to be deprecated).

### Managed runtime updates

* By default, Lambda applies runtime updates automatically using a two-phase rollout.
  - In the first phase, AWS applies the new runtime version whenever you create or update a function.
  - In the second phase, for any function that has Auto enabled and hasn't been updated, AWS applies the new runtime version.
  - The overall duration of the rollout process varies according to the severity of any patches.

* You can configure to use update manually, and set the runtime to a specific version.
  - In the event that a new runtime version is incompatible with your existing function, you can roll back to an earlier runtime version.
  - There is no time limit on how long you can use any runtime version, however do note that deprecated runtimes are not patched.

* AWS publishes each new runtime version as a container image.

* To update your own container image, you must create a new container image from the AWS published container image and redeploy your function.

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

* [Lambda runtimes](https://docs.aws.amazon.com/lambda/latest/dg/lambda-runtimes.html)