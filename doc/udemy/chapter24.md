# Chapter 24. AWS Monitoring & Audit: CloudWatch, CloudTrail & Config

<!-- TOC -->

- [Chapter 24. AWS Monitoring & Audit: CloudWatch, CloudTrail & Config](#chapter-24-aws-monitoring--audit-cloudwatch-cloudtrail--config)
    - [AWS Config](#aws-config)
        - [Config Rules](#config-rules)
        - [Config Use Cases](#config-use-cases)

<!-- /TOC -->

---
## AWS Config

When you turn on AWS Config, it first discovers the supported AWS resources that exist in your account and generates a configuration item for each resource in the region. AWS Config keeps track of all changes to your resources by invoking the Describe or the List API call for each resource in your account.

For example, removing an egress rule from a security group causes it to invoke a Describe API call on the security group. AWS Config then invokes a Describe API call on all of the instances associated with the security group. The updated configurations are recorded as configuration items and delivered in a configuration stream to an S3 bucket.

### Config Rules

You can choose to use AWS Managed Config rules, or make your own custom Config rules. Rules can be evaluated for each configuration change, or triggered at regular time intervals.

Note that Config rules do not prevent actions from happening (no deny), but you can automate remediation of non-compliant resources using SSM automation documents.

If you are using AWS Config rules, it continuously evaluates your AWS resource configurations for desired settings. Each rule is associated with an AWS Lambda function, which contains the evaluation logic for the rule.

The function returns the compliance status of the evaluated resources. If a resource violates the conditions of a rule, AWS Config flags the resource and the rule as noncompliant. When the compliance status of a resource changes, AWS Config sends a notification to your SNS topic, or send an event to EventBridge to indirectly trigger a Lambda function.

### Config Use Cases

* Check for unrestricted SSH access in your security groups

* Check S3 buckets for public access.

* How ALB configuration changed over time.

* Certificates managed by ACM has an expiration of less than 30 days.

* Check EBS of instances for type, e.g. gp2.