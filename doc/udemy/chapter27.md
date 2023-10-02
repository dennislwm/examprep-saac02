<!-- TOC -->

- [Single Region Multi-VPC Connectivity](#single-region-multi-vpc-connectivity)
    - [General Best Practices](#general-best-practices)
    - [Partially or Fully Meshed Network VPC Peering](#partially-or-fully-meshed-network-vpc-peering)
    - [VPCs connected with AWS Direct Connect](#vpcs-connected-with-aws-direct-connect)
    - [Transit VPC](#transit-vpc)
- [Direct Connect & Direct Connect Gateway](#direct-connect--direct-connect-gateway)
    - [Direct Connect Virtual Interface](#direct-connect-virtual-interface)
- [References](#references)

<!-- /TOC -->](#references)

<!-- /TOC -->

<!-- /TOC -->

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
## References

* [AWS Single Region Multi-VPC Connectivity](https://d0.awsstatic.com/aws-answers/AWS_Single_Region_Multi_VPC_Connectivity.pdf)

* [Which type of Direct Connect virtual interface should I use to connect different AWS resources?](https://repost.aws/knowledge-center/public-private-interface-dx)

* [Connect to the internet using an internet gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html)