# Chapter 6. Elastic Compute Cloud (EC2)

<!-- TOC -->

- [Chapter 6. Elastic Compute Cloud EC2](#chapter-6-elastic-compute-cloud-ec2)
  - [EC2 Overview](#ec2-overview)
    - [EC2 Pricing Options](#ec2-pricing-options)
      - [On-Demand Instances](#on-demand-instances)
      - [Reserved Instances](#reserved-instances)
      - [When to Use Spot Instances](#when-to-use-spot-instances)
      - [Dedicated Hosts](#dedicated-hosts)
  - [Using Roles](#using-roles)
  - [Security Groups and Bootstrap Scripts](#security-groups-and-bootstrap-scripts)
  - [EC2 Metadata and User Data](#ec2-metadata-and-user-data)
  - [Networking with EC2](#networking-with-ec2)
  - [Optimizing with EC2 Placement Groups](#optimizing-with-ec2-placement-groups)
  - [Solving Licensing Issues with Dedicated Hosts](#solving-licensing-issues-with-dedicated-hosts)
  - [Timing Workloads with Spot Instances and Spot Fleets](#timing-workloads-with-spot-instances-and-spot-fleets)
  - [Lab 6. Create and Assume Roles in AWS](#lab-6-create-and-assume-roles-in-aws)
    - [Create the Correct S3 Restricted Policies and Roles](#create-the-correct-s3-restricted-policies-and-roles)

<!-- /TOC -->

---
## EC2 Overview

### EC2 Pricing Options

1. On-Demand Instances: Pay by the hour or the second, depending on the type of instance you run

2. Reserved Instances: Contract for 1 or 3 years, up to 72% discount (longer period and higher upfront payment, a higher discount)

3. Spot Instances: Purchase unused capacity, up to 90% discount (prices fluctuate based on supply and demand)

4. Dedicated Host: Physical server for your use, most expensive option

> Exam Tip: For each scenario, what is the best EC2 pricing option?

#### On-Demand Instances

* Flexible: Low cost and flexibility of instances without any upfront payment or long-term contract
* Short-Term: Applications with short-term, spiky, or unpredictable workloads that cannot be interrupted
* Testing the Water: Applications being developed or tested on instances for the first time

#### Reserved Instances

* Regional Level: Reserved instances operate at a regional level

* Predictable Usage: Applications with steady state or predictable usage

* Specific Capacity Requirements: Applications that require reserved capacity

* Pay up Front: You can make upfront payments to reduce the total computing coss even further

* Standard Reserved Instances: Up to 72% off the on-demand price (highest discount if paid in full upfront with a 3-year contract)

* Convertible Reserved Instances: Up to 54% off the on-demand price (has the option to change to a different Reserved Instance type of equal or greater value)

* Scheduled Reserved Instances: Launch within the time window you define. Match your capacity reservation to a predictable recurring schedule that only requires a fraction of a day, week, or month.

* Super Flexible: Not only EC2, but this includes serverless like Lambda and Fargate as well

#### When to Use Spot Instances

* Flexible: Applications that have flexible start and end times, i.e. you should not use Spot instances for a 24/7 web server

* Urgent Capacity: Urgent need for large amount of additional computing capacity, e.g. image rendering, algo trading

* Cost Sensitive: Applications that are only feasible at very low compute prices, e.g. batch jobs executed during non-working hours

#### Dedicated Hosts

* Compliance: Regulatory requirements that may not support multi-tenant virtualization

* Licensing: Licensing that does not support multi-tenancy or cloud deployments

* On-Demand Host: Can be purchased on-demand (hourly)

* Reserved Host: Can be purchased as a Reserved Host, up to 70% off the on-demand price

## Using Roles

## Security Groups and Bootstrap Scripts

## EC2 Metadata and User Data

## Networking with EC2

## Optimizing with EC2 Placement Groups

## Solving Licensing Issues with Dedicated Hosts

## Timing Workloads with Spot Instances and Spot Fleets

---
## Lab 6. Create and Assume Roles in AWS

### Create the Correct S3 Restricted Policies and Roles

1. Create 