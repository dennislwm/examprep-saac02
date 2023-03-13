# Chapter 17. Automation

<!-- TOC -->

- [Chapter 17. Automation](#chapter-17-automation)
  - [Why Do We Automate](#why-do-we-automate)
    - [Benefits of Automation](#benefits-of-automation)
    - [Exam Tips](#exam-tips)
  - [CloudFormation](#cloudformation)
    - [Exam Tips](#exam-tips)
  - [Elastic Beanstalk](#elastic-beanstalk)
    - [Exam Tips](#exam-tips)
  - [Systems Manager](#systems-manager)
    - [Feature Overview](#feature-overview)
    - [Exam Tips](#exam-tips)
  - [Lab 17.1. Getting Started with CloudFormation](#lab-171-getting-started-with-cloudformation)
    - [Introduction](#introduction)
    - [Create a CloudFormation Stack](#create-a-cloudformation-stack)
    - [Delete CloudFormation Stack](#delete-cloudformation-stack)

<!-- /TOC -->

---
## Why Do We Automate

Manual steps are error prone. Lazy is a good thing. 

Automate yourself out of a job.

### Benefits of Automation

* *Time* - we have more time for other tasks

* *Security* - easier to prevent security incidents

* *Consistency* - you get the same results every single time

* *Speed* - you can get the result faster

* *Immutable* - you can easily create and dsetroy your resource as needed

### Exam Tips

If you see a scenario that allows you to replace a manual step with automated tools, make sure what you're selecting automates the entire process and not just a portion of it.

---
## CloudFormation

**CloudFormation** is a declarative programming language. It supports either JSON or YAML formatting, however only YAML support comments.

### Exam Tips

* Hard-coded values can be the reason templates fail to create. You should use the **Mappings** section to auto-fill these values based on a condition.

* If it finds an error, CloudFormation rolls back to the last known good state.

* CloudFormation makes the same API calls you make manually.

---
## Elastic Beanstalk

**Platform as a Service (PaaS)** is a single-stop application deployment model. You bring your code, and the provider builds everything for you, deploys your application, and then manages it going forward.

**Elastic Beanstalk** is an AWS PaaS tool that:
- *Automation* - automates all your deployments, while you can templatize your environment
- *Deployment* - handle all your deployments, while you upload your code
- *Management* - handle building out the instance for you

### Exam Tips

* Supports containers, Windows, and Linux applications. In general, it is only for simpler web applications.

* It is not serverless. Elastic Beanstalk is creating and managing standard EC2 architecture.

---
## Systems Manager

**Systems Manager** is a suite of tools designed to let you view, control, and automate both your AWS architecture and on-premises resources.

### Feature Overview

* *Automation Documents* - control or configure your instances or AWS resources. It is also known as Runbooks.

* *Run Command* - execute commands on your hosts via SSM agent.

* *Patch Manager* - manages your application versions.

* *Parameter Store* - securely store your secret values.

* *Hybrid Activations* - control your on-premises architecture.

* *Session Manager* - remotely connect and interact with your architecture.

### Exam Tips

* Systems Manager will rarely be called out by name. Instead, the names of the features will be used, such as *Parameter Store*, *Session Manager*, *Automation Document* etc.

* Systems Manager is free, however there is a small fee for managing on-premises architecture.

* *Automation Documents* are usable by **AWS Config** to enforce architecture state.

---
## Lab 17.1. Getting Started with CloudFormation

<details>
<summary>Click here to start Lab 17.1.</summary>

### Introduction

This lab provides an introduction to CloudFormation, using it to create and update a number of S3 buckets. By the end of this lab, you will be comfortable using CloudFormation and can begin experimenting with your own templates.

### Create a CloudFormation Stack

1. Create a file `createstack.json` with the following code:

```json
{
    "Resources": {
        "catpics": {
            "Type": "AWS::S3::Bucket"
        }
    }
}
```

2. In the AWS console, navigate to CloudFormation > Create stack > **With new resources (standard)**.
3. Select **Template is ready**.
4. Select **Upload a template file**.
5. Click **Choose file**.
6. Browse to the `createstack.json` file that you created and saved.
7. Select and upload it, and click **Next**.
8. Name the stack `cfnlab`.
9. Click **Next**.
10. Leave all available stack options at the defaults, and click **Next**.
11. Review your selections and click **Submit**.
12. Navigate to S3. We didn't specify a name in the `json` file for this bucket, so AWS names it with the `<STACK_NAME>-<LOGICAL_VOLUME-NAME>-<RANDOM_STRING>` format. Yours will be `cfnlab-catpics-<RANDOM_STRING>`.

### Delete CloudFormation Stack

1. Navigate to CloudFormation > Select `cfnlab`.
2. Click **Delete**.
3. In the dialog box, click **Delete stack**.
4. Once it is completed, navigate to S3. You should see the `cfnlab-catpics-<RANDOM_STRING>` bucket gone.

</details>