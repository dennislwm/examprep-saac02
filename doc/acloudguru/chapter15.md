# Chapter 15. Serverless Architecture

<!-- TOC -->

- [Chapter 15. Serverless Architecture](#chapter-15-serverless-architecture)
  - [Serverless Overview](#serverless-overview)
    - [Benefits](#benefits)
    - [Exam Tips](#exam-tips)
  - [Computing with Lambda](#computing-with-lambda)
    - [Building a Function](#building-a-function)
    - [Exam Tips](#exam-tips)
  - [AWS Serverless Application Repository](#aws-serverless-application-repository)
    - [Publish and Deploy](#publish-and-deploy)
  - [Container Overview](#container-overview)
    - [Container Terminology](#container-terminology)
    - [Exam Tips](#exam-tips)
  - [Running Containers in ECS or EKS](#running-containers-in-ecs-or-eks)
    - [Why Elastic Container Service ECS?](#why-elastic-container-service-ecs)
    - [Using Kubernetes K8s](#using-kubernetes-k8s)
    - [ECS vs EKS](#ecs-vs-eks)
    - [Exam Tips](#exam-tips)
  - [Removing Servers with Fargate](#removing-servers-with-fargate)
    - [What is Fargate?](#what-is-fargate)
    - [EC2 vs Fargate as the underlying resource for AWS ECS or AWS EKS](#ec2-vs-fargate-as-the-underlying-resource-for-aws-ecs-or-aws-eks)
    - [Exam Tips](#exam-tips)
  - [Amazon EventBridge CloudWatch Events](#amazon-eventbridge-cloudwatch-events)
    - [Creating a Rule](#creating-a-rule)
    - [Exam Tips](#exam-tips)
  - [Storing Custom Docker Images in Amazon Elastic Container Registry ECR](#storing-custom-docker-images-in-amazon-elastic-container-registry-ecr)
    - [ECR Components](#ecr-components)
    - [ECR Features](#ecr-features)
      - [Lifecycle Policies](#lifecycle-policies)
      - [Image Scanning](#image-scanning)
      - [Sharing](#sharing)
      - [Cache Rules](#cache-rules)
      - [Tag Mutability](#tag-mutability)
    - [ECR Integration](#ecr-integration)
    - [Exam Tips](#exam-tips)
  - [Using Open-Source Kubernetes in Amazon EKS Distro EKS-D](#using-open-source-kubernetes-in-amazon-eks-distro-eks-d)
    - [Exam Tips](#exam-tips)
  - [Orchestrating Containers Outside AWS Using Amazon ECS Anywhere and Amazon EKS Anywhere](#orchestrating-containers-outside-aws-using-amazon-ecs-anywhere-and-amazon-eks-anywhere)
  - [Auto Scaling Databases On Demand with Amazon Aurora Serverless](#auto-scaling-databases-on-demand-with-amazon-aurora-serverless)
  - [Using AWS X-Ray for Application Insights](#using-aws-x-ray-for-application-insights)
  - [Deploying GraphQL Interfaces in AWS AppSync](#deploying-graphql-interfaces-in-aws-appsync)
  - [Lab 15.1. Triggering AWS Lambda from Amazon SQS](#lab-151-triggering-aws-lambda-from-amazon-sqs)
    - [Introduction](#introduction)

<!-- /TOC -->

---
## Serverless Overview

### Benefits

* *Ease of use* - AWS handles almost everything for us outside of our code.

* *Event based* - Serverless compute resources can be brought online in response to an event happening.

* *Billing model* - PAYG as you pay for your provisioned resources and the length of runtime.

### Exam Tips

* It is almost always better to select an answer on the text that uses Lambda or containers rather than a traditional operating system.

---
## Computing with Lambda

**Lambda** is a serverless compute service that lets you run code without provisioning or managing the underlying servers.

### Building a Function

* *Runtime* - you'll need to pick from an available runtime or bring your own. This is the environment your code will run in.

* *Permissions* - if your function needs to make an AWS API call, you'll need to attach a role.

* *Networking* - you can optionally define the VPC, subnet, and security groups your functions are a part of.

* *Resources* - you define the amount of available memory etc.

* *Trigger* - you define a trigger that will kick Lambda off if that event occurs.

### Exam Tips

* If you need to automatically do anything that isn't built in to AWS, the answer is most likely going to be to use Lambda to achieve that.

* Lambda can run inside or outside a VPC.

---
## AWS Serverless Application Repository

* *Serverless apps* - allows users to easily find, deploy, or publish their own serverless applications.

* *Sharing is caring* - ability to privately share applications within orgs or publicly.

* *Manifest* - Upload your application code and a manifest file, known as the **AWS SAM** template.

* *Integrations* - deeply integrated with **Lambda** service.

### Publish and Deploy

* *Publish* - publishing apps makes them available for other to find and deploy. Define apps with **AWS SAM** templates, which are similar to CloudFormation templates.

> Note: Published apps are set to private by default and are available only to your AWS account. You must explicitly share if desired.

* *Deploy* - find and deploy published applications. Browse public apps without needing an AWS account, or within the AWS Lambda console.

---
## Container Overview

A container is a standard unit of software that packages up code and all its dependencies, so the application runs quickly and reliably from **one computing environment to another**.

### Container Terminology

| Term       | Description                                                                                                    |
|------------|----------------------------------------------------------------------------------------------------------------|
| Dockerfile | Text document that contains all the commands or instructions that will be used to build an image.              |
| Image      | Immutable file that contains the code, libraries, dependencies, and config files needed to run an application. |
| Registry   | Stores Docker images for distribution. They can be both private and public.                                    |
| Container  | A running copy of the image that has been created.                                                             |

Example Dockerfile:

```Dockerfile
FROM centos:7

# Install dependencies
RUN yum update -y
RUN yum install httpd -y

# Install app
RUN rm -rf /var/www/html/*
ADD code /var/www/html
RUN ln -sf /dev/stdout /var/log/httpd/access_log
RUN ln -sf /dev/stderr /var/log/httpd/error_log

EXPOSE 80

CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]
```

### Exam Tips

* High Level - You won't have to dive into the specifics of a Dockerfile.
* Ease of Use - Containers help you easily migrate from on-premises to AWS.
* Dev vs Prod - Dev is prod, and prod is dev (but it's a good thing with containers)

---
## Running Containers in ECS or EKS

Orchestration of containers can be an issue for a large number of containers.

### Why Elastic Container Service (ECS)?

* AWS ECS is a proprietary managed service to orchestrate running containers.

* AWS ECS integrates with ELB as containers are appropriately registered with load balances as they come online and go offline.

* AWS ECS integrates with IAM Roles as containers can have individual roles attached to them.

* AWS ECS is extremely easy to setup and scale to handle any workload.

### Using Kubernetes (K8s)

* K8s is an open source alternative to orchestrate running containers.

* K8s can be used on-premises and in the cloud.

* AWS EKS is a managed service to orchestrate running containers using K8s.

### ECS vs EKS

* AWS ECS is best when you're all in on AWS and looking for ease of use.

* AWS EKS is best when you're not all in on AWS but there is more work to configure and integrate with AWS.

### Exam Tips

* AWS ECS is preferred, and if you see containers mentioned, you should think ECS.

* The only exception to this is if the question is asking about K8s or open source, or running container on-premises.

> Note: AWS ECS on-premises is generally available.

---
## Removing Servers with Fargate

AWS ECS or AWS EKS orchestration of containers can be an issue for a large number of containers, as the underlying resources are EC2 instances.

### What is Fargate?

* AWS Fargate is a serverless compute engine for containers that works with both AWS ECS and AWS EKS.

* AWS owns and manages underlying infrastructure for AWS ECS or AWS EKS.

* AWS Fargate requires the use of AWS ECS or AWS EKS.

### EC2 vs Fargate as the underlying resource for AWS ECS or AWS EKS

| EC2                                     | Fargate                                  |
|-----------------------------------------|------------------------------------------|
| You are responsible for underlying OS   | No operating system access               |
| EC2 pricing model                       | Pay based on resources allocated and ran |
| Long-running containers                 | Short-running containers                 |
| Multiple containers share the same host | Isolated environments                    |

### Exam Tips

* Fargate may be mentioned without ECS or EKS reference in the exam.

* Fargate is more expensive, but easier to use, than EC2.

* Lambda is for a single function that has a time limit of 15 minutes.

* EC2 > Fargate > Lambda for containers running a long period of time.

---
## Amazon EventBridge (CloudWatch Events)

AWS EventBridge (formerly known as CloudWatch Events) is a **serverless event bus** that allows you to pass events from a source to an endpoint. Essentially, it's the glue that holds your serverless application together.

### Creating a Rule

1. Define Pattern - Should the rule to be invoked based on an event or a schedule?

2. Select Event Bus - Is this going to be an AWS-based event, custom event, etc?

3. Select Target - What happens after the event, does it trigger a Lambda function, post to an SQS queue, send an email, etc?

4. Tag - You need to tag everything.

5. Sit Back - Wait for the event to happen, or trigger the event manually.

### Exam Tips

* Any AWS API call that happens in AWS can alert a Lambda function, or a variety of different endpoints.

* There's more to EventBridge than rules, but it won't be on the exam.

* This is the fastest way to respond to things happening in your environment through AWS API.

---
## Storing Custom Docker Images in Amazon Elastic Container Registry (ECR)

AWS ECR is a managed container image registry that offers secure, scalable, and reliable infrastructure.

* Private container image repositories with resource-based permissions via IAM.

* Supports Open Container Initiative (OCI) images, artifacts, and Docker images.

> Note: AWS ECR Public is a similar service for public image repositories.

### ECR Components

* Registry - one or more private registries provided to each AWS account.

* Authorization Token - required for pushing and pulling images to and from registries.

* Repository - contains all your images and artifacts.

* Repository Policy - controls all access to repos and images.

* Image - container images that get pushed to and pulled from your repository.

### ECR Features

#### Lifecycle Policies

* Helps management of images in your repositories.

* Defines rules for cleaning up unused images.

* Ability to test your rules before applying them.

#### Image Scanning

* Helps identify software vulnerabilities in your container images.

* Repositories can be set to scan on push.

* Retrieve results of scans for each image.

#### Sharing

* Cross-region support.

* Cross-account support.

* Configured per repository and per region. Each registry is regional for each account.

#### Cache Rules

* Pull through cache rules allow for caching public repos privately.

* AWS ECR periodically reaches out to check current caching status.

#### Tag Mutability

* Prevents image tags from being overwritten

* Configured per repository.

### ECR Integration

* BYO images to leverage AWS ECR.

* AWS ECS or EKS uses container images in ECR.

* Amazon Linux containers can be used locally for software development.

### Exam Tips

Keywords to look for questions related to AWS ECR are:
* managed container image registry
* OCI repositories
* image integration with AWS ECS or AWS EKS

---
## Using Open-Source Kubernetes in Amazon EKS Distro (EKS-D)

AWS EKS-D is a K8s distribution based on and used by AWS EKS, where the only difference is EKS-D is fully managed by you, unlike AWS EKS, which is managed by AWS.

* Same versions and dependencies used in AWS EKS.

* Run EKS-D on-premises or in the cloud.

* You are responsible for upgrading and managing your platform.

### Exam Tips

Keywords to look for questions related to AWS EKS-D are:
* self-managed K8s deployment
* clusters outside of AWS managed services or on-premises

---
## Orchestrating Containers Outside AWS Using Amazon ECS Anywhere and Amazon EKS Anywhere


---
## Auto Scaling Databases On Demand with Amazon Aurora Serverless


---
## Using AWS X-Ray for Application Insights


---
## Deploying GraphQL Interfaces in AWS AppSync


---
## Lab 15.1. Triggering AWS Lambda from Amazon SQS

<details>
<summary>Click here to start Lab 15.1.</summary>

### Introduction

</details>