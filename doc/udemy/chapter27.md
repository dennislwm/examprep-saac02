# Chapter 27. Networking - VPC

<!-- TOC -->

- [Chapter 27. Networking - VPC](#chapter-27-networking---vpc)
    - [Internet Gateways and Route Tables](#internet-gateways-and-route-tables)
        - [Internet Gateway IG](#internet-gateway-ig)
        - [NAT Devices](#nat-devices)
    - [Egress-Only Internet Gateway IG](#egress-only-internet-gateway-ig)
    - [Single Region Multi-VPC Connectivity](#single-region-multi-vpc-connectivity)
        - [General Best Practices](#general-best-practices)
        - [Partially or Fully Meshed Network VPC Peering](#partially-or-fully-meshed-network-vpc-peering)
        - [VPCs connected with AWS Direct Connect](#vpcs-connected-with-aws-direct-connect)
        - [Transit VPC](#transit-vpc)
    - [Direct Connect & Direct Connect Gateway](#direct-connect--direct-connect-gateway)
        - [Direct Connect Virtual Interface](#direct-connect-virtual-interface)
    - [References](#references)

<!-- /TOC -->

---
## Internet Gateways and Route Tables

### Internet Gateway (IG)

An IG is a scalable, redundant, and HA VPC component that allows communication between your VPC subnets and the internet. An IG provides a target in your VPC route tables for internet-routable traffic, and it also performs network address translation (NAT).

If a subnet is associated with a route table that has a route to an IG, it's known as a public subnet, otherwise, it is known as a private subnet.

In your public subnet's route table, you can specify an outbound route for the IG to all destinations not explicitly known to the route table, i.e. `0.0.0.0/0`. Alternatively, you can specify the route to a narrower range, such as your company's public IPs outside of AWS.

To enable communication over the internet, your instance must have a public IP. The IG logically provides the one-to-one NAT on behalf of your instance, so that when outbound traffic goes to the internet, the reply address field is set to the public IP of your instance, and not its private IP. Conversely, inbound traffic that is destined for your instance's public IP has its destination address translated into the instance's private IP before the traffic is delivered to the VPC subnet.

### NAT Devices

You can use a NAT device to allow resources in a private subnet to connect to the internet, other VPCs, or on-premises network. These instances can communicate with services outside the VPC, but they cannot receive unsolicited connection requests.

You can use AWS managed NAT device, called a NAT Gateway, or you can create your own NAT device on an EC2 instance, called a NAT instance.

NAT devices are not supported for IPv6 traffic, use an egress-only Internet Gateway instead.

---
## Egress-Only Internet Gateway (IG)

An egress-only IG is a scalable, redundant HA VPC component that allows outbound communication over IPv6 from instances in your VPC subnets to the internet, and prevents the internet from initiating an IPv6 connection with your instances.

Note: To enable outbound-only internet communication over IPv4, use a NAT gateway instead.

An egress-only IG is stateful as it forwards traffic from the instances in the VPC subnet to the internet, and then sends the response back to the instances.

* You cannot associate a security group with an egress-only IG (attach them to your instances instead).

* You cannot use NACL to control traffic to and from your subnet for which the egress-only IG routes traffic.

---
## Single Region Multi-VPC Connectivity

Customers can create multiple VPCs within the same region or in different regions, in the same account or in different accounts. More often than not, these different VPCs need to communicate privately and securely with one another for sharing data and applications.

We will take a look at high-level connectivity options for multiple VPCs within the same region using VPC peering or Direct Connect connections. It includes best practices and guidance, and outlines the most commonly used multiple-VPC connection configurations within a region.

We describe three possible solutions for connective VPCs:

* Partially or Fully Meshed Network (VPC Peering)

* VPCs connected with AWS Direct Connect

* Transit VPC

### General Best Practices

* Ensure that your VPC network ranges (CIDR blocks) do not overlap.

* Ensure you implement a highly available and scalable design.

* Connect only those VPCs that really need to communicate with each other.

* Consider your data-transfer and cost-optimization needs.

### Partially or Fully Meshed Network (VPC Peering)

This approach allows customers who have a central VPC that contains shared resources to connect VPCs as appropriate.

This hub-and-spoke formation is best suited for customers with the following requirements:

* They do not require full connectivity between all of their VPCs.

* They would benefit from a central VPC to host shared services, such as Active Directory etc.

* They have multiple VPCs that need access to shared resources but do not need access to each other.

* They require fewer than 125 peering connections per VPC.

To enable flow of traffic between VPCs, the central VPC's route table must contain a route that points to all or part of the CIDR block of each spoke VPC. Similarly, each spoke VPC must have a route that points to the IP address range of the hub VPC.

Note that Amazon VPC does not support directly routing from one VPC to another through an intermediary VPC's peered connections. For example, if VPC A is peered with VPC B, and VPC B is peered with VPC C, VPC A will not have access to VPC C unless a new peering connection is established between them.

Each peering connection requires modifications to the associated VPCs' route table and, as the number of VPCs grows, this can be difficult to maintain.

### VPCs connected with AWS Direct Connect

This approach is a good alternative for customers who need to connect a high number of VPCs (more than 125 connections per VPC) to a central VPC or to on-premises resources. For example, if VPC A and VPC B are both connected to an on-premises network using AWS Direct Connect, then the two VPCs can be connected to each other via AWS Direct Connect.

A single physical AWS Direct Connect connection to on-premises can be routed and divided into multiple logical connections, called virtual interfaces, one per VPC. On the AWS side, a Virtual Private Gateway (VGW) is configured for each VPC.

### Transit VPC

This approach uses customer-managed EC2 VPN instances in a dedicated transit VPC with an Internet gateway. The EC2 instances initiate the VPN connections and route traffic between multiple VPCs and shared-services VPCs.

The spoke VPCs can leverage VPC peering to circumvent the transit VPC, providing more scalable, direct access between VPCs.

The design requires the customer to deploy, configure, and manage EC2-based VPN appliances, therefore it is suited for customers who have already implemented a transit VPC and want to leverage it to manage more advanced connection types, such as inter-region connectivity etc.

Note that this design will generate additional data transfer charges for traffic traversing the transit VPC.

---
## Direct Connect & Direct Connect Gateway

### Direct Connect Virtual Interface

Direct Connect provides three types of virtual interfaces (VIFs):

* Public Virtual Interface - connect to AWS resources that are reachable by a public IP address, such as S3 etc.

* Private Virtual Interface - connect to AWS resources using their private IP addresses, such as EC2 instances etc.

* Transit Virtual Interface - connect to AWS resources using their private IP addresses through a transit gateway.

---
## Transit Gateway

* Transitive peering between thousands of VPCs and on-premises.

* Hub-and-spoke (star) connections across multiple regions.

* Route tables limit which VPC can talk with another VPC.

* Works with Direct Connect, VPN connections.

* Supports IP Multicast (not supported by any other AWS service).

* Increase Site-to-Site VPN bandwidth.

* Share one Direct Connect betweeen multiple AWS accounts.

### Equal-Cost Multi-Path (ECMP) Routing

ECMP is a routing strategy to increase bandwitch of Site-to-Site VPN connections to your AWS cloud. It allows the routing strategy to forward a packet over multiple best path.

If you connect on-premises VPN to an AWS virtual private gateway, you get a bandwidth of 1.25 Gbps. However, if you connect the same VPN to an AWS transit gateway, you get a bandwidth of 2.5 Gbps due to ECMP, and in addition you can have scale the bandwidths using ECMP.

---
## References

* [AWS Single Region Multi-VPC Connectivity](https://d0.awsstatic.com/aws-answers/AWS_Single_Region_Multi_VPC_Connectivity.pdf)

* [Which type of Direct Connect virtual interface should I use to connect different AWS resources?](https://repost.aws/knowledge-center/public-private-interface-dx)

* [Connect to the internet using an internet gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html)

* [Enable outbound IPv6 traffic using an egress-only internet gateway](https://docs.aws.amazon.com/vpc/latest/userguide/egress-only-internet-gateway.html)