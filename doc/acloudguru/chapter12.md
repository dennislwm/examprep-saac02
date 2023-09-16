# Chapter 12. High Availability and Scaling

<!-- TOC -->

- [Chapter 12. High Availability and Scaling](#chapter-12-high-availability-and-scaling)
  - [Horizontal vs Vertical Scaling](#horizontal-vs-vertical-scaling)
  - [Launch Templates and Configurations](#launch-templates-and-configurations)
  - [Auto Scaling Groups](#auto-scaling-groups)
    - [Auto Scaling Group Steps](#auto-scaling-group-steps)
    - [Auto Scaling Group Capacity Limits and Policies](#auto-scaling-group-capacity-limits-and-policies)
  - [Scaling Databases](#scaling-databases)
    - [Relational Databases](#relational-databases)
    - [Non-Relational Databases](#non-relational-databases)

<!-- /TOC -->

---
## Horizontal vs Vertical Scaling

|    Vertical Scaling     |      Horizontal Scaling      |
|:-----------------------:|:----------------------------:|
| Increase CPU and Memory | Increase number of instances |
|  No High Availability   |         Increase HA          |
|      Single Region      |       Multiple Regions       |

---
## Launch Templates and Configurations

A Launch template is a collection of all the required settings to launch an EC2 instance, so you can reuse the same settings for multiple instances.

Launch template supercedes launch configurations.

|         Templates          |      Configurations      |
|:--------------------------:|:------------------------:|
| More than just autoscaling |   Only for autoscaling   |
|    Supports versioning     |        Immutable         |
|      More granularity      |     Limited options      |
|   Can override settings    | Cannot override settings |
|    Networking settings     |  No networking settings  |
|      AWS recommended       |      Try not to use      |

Don't specify networking details in your launch template if you intend to use it for an autoscaling group.

---
## Auto Scaling Groups

An Auto Scaling Group (ASG) contains a collection of EC2 instances that are treated as a collective group for purposes of scaling and management.

### Auto Scaling Group Steps

1. Select your Launch template or configuration.

2. Select your networking details, e.g. VPC, multiple AZs etc.

3. Optional add an Elastic Load Balancer, and enable health checks, in front of your instances.

4. Set the desired scaling policies, e.g. minimum, maximum and desired.

5. Set SNS to notify you when a scaling occurs.

### Auto Scaling Group Capacity Limits and Policies

ASG Capacity Limits:

* Minimum - set at least 2 instances for HA, although it is possible to set zero.

* Maximum - highest number of instances that you'll ever scale up.

* Desired - set current number of instances, within the range of minimum and maximum.

You could set one instance for each of the minimum, maximum and desired capacity limits, if you have a legacy application that requires HA.

Auto Scaling Policies:

* Step Scaling - For example, you can set scaling policies based on memory.
  - Scaling in (increase instances) conservatively:
    - Between 60-80% memory usage, add 10 instances.
    - Between 80-100% memory usage, add 15 instances.
  - Scaling out (decrease instances) aggressively:
    - Between 20-40% memory usage, terminate 10 instances.
    - Between 0-20% memory usage, terminate 15 instances.

* Warm-Up and Cooldown Period - ASG will wait for the warm-up / cooldown period when scaling to avoid trashing.
  - Warm-Up - delay instances from being placed behind LB, failing health check, and being scaled in.
  - Cooldown - pauses ASG for a set amount of time to avoid runaway scaling events.

* Scaling Types
  - Reactive Scaling - You're playing catch up with the load as you determine if you need to create more resources post-load.
  - Scheduled Scaling - You have a scheduled workload, and you provision your resources before they are needed.
  - Predictive Scaling - AWS uses its ML to determine when you'll need to scale, where they are reevaluated every 24 hours to create a forecast for the next 48.

* CloudWatch - AWS alert tool for ASG to scale your instances automatically.

---
## Scaling Databases

### Relational Databases

There are five ways to scale relational databases:

* Vertical scaling - increase memory and CPU, which causes downtime.

* Storage scaling - can increase storage, but not decrease.

* Horizontal scaling - replicas for read only operations (increase HA in multiple regions).

* Aurora scaling - offload scaling to AWS managed RDS.

* Migration scaling - change to DynamoDB for AWS managed NoSQL database.

### Non-Relational Databases

There are two read/write capacity settings for DynamoDB, where AWS manages scaling for you:

* Provisioned - Generally predictable workload, where you will need to set upper and lower scaling bounds based on your past experience, and most cost-effective model.

* On-Demand - Sporadic workload, with no user settings, but you pay a small amount of money per read and write (less cost-effect).

AWS allows you to switch capacity settings for an existing DynamoDB database, but you can do it only once per day.