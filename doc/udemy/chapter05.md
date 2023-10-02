# Chapter 5. EC2 Fundamentals

<!-- TOC -->

- [Chapter 5. EC2 Fundamentals](#chapter-5-ec2-fundamentals)
    - [Spot Instances](#spot-instances)
        - [Service-Linked Role for Spot Instances Requests](#service-linked-role-for-spot-instances-requests)
    - [Spot Fleet](#spot-fleet)

<!-- /TOC -->

---
## Spot Instances

To use Spot instances, you create a request that includes:

* desired number of instances
* instance type
* AZ

Amazon EC2 waits until your request can be fulfilled or until you cancel the request. A persistent request type means that the request is opened again when Amazon EC2 interrupts a Spot instance.

### Service-Linked Role for Spot Instances Requests

Amazon EC2 uses the service-linked role named `AWSServiceRoleForEC2Spot` to launch and manage Spot Instances on your behalf. The permissions granted by the role are `ec2:DescribeInstances`, `ec2:StopInstances`, and `ec2:StartInstances`.

---
## Spot Fleet

A Spot Fleet is a set of Spot instances and optionally On-Demand instances that is launched based on criteria that you specify. The Spot Fleet launches Spot instances to meet the target capacity for the fleet.

There are two types of Spot Fleet requests:

* Request - If capacity is diminished because of Spot interruptions, the fleet does not attempt to replenish Spot instances.

* Maintain - Capacity is maintained automatically by replenishing interrupted Spot instances.