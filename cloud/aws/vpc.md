---
description: Virtual Private Cloud
---

# 🔏 VPC

#### Resources:

* [Subnet Calculator](https://www.site24x7.com/tools/ipv4-subnetcalculator.html)
* [AWS VPC Docs](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)

### VPC Overview

* We can have multiple VPC's in AWS region (There is a soft limit of 5 VPC's per region)
* Maximum of 5 CIDR's can be assigned for a VPC
* Only private IP addresss ranges are allowed
  * CIDR Limit:
    * Min Size is /28 (16 IP addr)
    * Max size is /16 (65536 IP addr)

{% hint style="info" %}
VPC CIDR should not overlap with other networks
{% endhint %}

### Subnet

Subnet is sub-range of IPv4 address.  By default AWS has 3 subnets (with 3 IPv4 CIDR) across 3 AZ's for high availability.

Each subnet has a Route Table & Network ACL

AWS Reserves 5 IP addresses (first 4 & last 1) in each subnet&#x20;

Eg: (10.0.0.0/24)

1. 10.0.0.0 - Network Address
2. 10.0.0.1 - Reserved by AWS for the VPC Router
3. 10.0.0.2 -  Reserved by AWS for mapping to Amazon provided DNS
4. 10.0.0.3 - Reserved by AWS for future use
5. 10.0.0.255 - Reserved network broadcast address. AWS does not support broadcast in a VPC.

> If EC2 instance launch is failing in a subnet it could be becase there are no IPv4 addresses available in the subnet. The solution would be to **create a new IPv4 CIDR** in the subnet.

### Internet Gateway

An internet gateway is a horizontally scaled, redundant, and highly available VPC component that allows communication between your VPC and the internet.

* One VPC can only be attached to One IGW & vice versa.&#x20;
* Support IPv4 & IPv6&#x20;
* Internet resources can initiate a connection to resources in your subnet using the public IPv4 or IPv6 address
* Internet gateway is free of charge (Outbound traffic is charged)

Internet gateway enables resources in your **public subnets** (such as EC2 instances) to connect to the internet if the resource has a public IPv4 address or an IPv6 address.

{% hint style="info" %}
Internet gateway's do not allow internet access on their own. [Route tables](https://app.gitbook.com/s/d8qGZNNEP3vhnf91dxDT/\~/changes/fq73XzQhOre23mAFHreJ/aws/vpc#route-table) must be configured for internet access.
{% endhint %}

### Route Table

Define & configure traffic route to & from the subnet.&#x20;

* Public route tables are mapped to public subnets and private route tables are mapped to private subnets.
* Routes must be configred for Route tables.&#x20;

Eg: For a public route table any traffic that does not match the subnet (local ip) should be going to the IGW.&#x20;

### Bastion Host

Allow users to access EC2 instance on a private subnet through the internet.

A bastion host is a EC2 instance in a public subnet named bastion host which is connected to all other private subnets.&#x20;

Bastion host will have it's own security group, it must allow inbound from the internet on port 22.&#x20;

EC2 instances in the private subnet must allow port 22 inbound from the private ip/security group of the bastion host

### NAT Instances (Outdated)

Allow EC2 instances in a private subnet to connect to the internet.&#x20;

NAT instances must be launched in a public subnet.&#x20;

EC2 setting (Source/destination check) must be disabled. Because the source & destination for network packets will be different

NAT instance must have a elastic IP attached.&#x20;

Route tables must be configured to route traffic from private subnets to the NAT instance. Specipic protocols also must be enabled. Eg: icmp for ping

NAT Instance can be used as a bastion host.&#x20;

### NAT Gateway

Enable instances present in a private subnet to help connect to the internet or AWS services.

NAT gateway makes sure that the internet doesn’t initiate a connection with the instances. NAT Gateway service is a fully managed service by AWS.&#x20;

* Pay per hour for usage and bandwidth. Supports upto 45 Gbps bandwidth.&#x20;
* NATGW is created in a specifc AZ, it uses a elastic IP.&#x20;
* Can't be used by a EC2 instance in the same subnet.&#x20;
* NAT Gateway cannot be used as a bastion host.&#x20;
* Routes must be edited in the route table to allow traffic to the internet gateway.

{% hint style="info" %}
Traffic route: IGW (Private Subnet -> NATGW -> IGW)

NATGW cannot work without an internet gateway.&#x20;
{% endhint %}

### Network ACL (NACL)

In an incoming request, the request will go through the NACL before going to the subnet.&#x20;

NACL is stateless i.e. both inbound & outbound rules are evaluated.

NACL is a firewall for the subnet. They are great at blocking a specific IP at the subnet level.&#x20;

There is 1 NACL per subnet by default. The **default NACL accepts everything** inbound/outbound while **newly created NACLs will deny** everything.&#x20;

NACL Rules:&#x20;

* Rules have a number (1-32766) -> Lower numbers have higher precedence
* First rule match will drive decision
* The last rule is asterisk (\*) & denies a request in case of no rule match.
* AWS recommends adding rules by increment of 100.

### Security Groups

Security group is stateful, i.e whatever traffic is accepted is allowed to go out. Only inbound traffic is evaluated.

Security group operates at the instance level and supports allow rules only.&#x20;

### VPC Perring

Privately connect to 2 VPCs using AWS network

VPC network CIDR should not overlap.&#x20;

VPC peering connection is not transitive i.e. Peering must be enabled for each VPC that needs to communicate with each other.

Route tables must be updated in each VPCs subnet to ensure EC2 instances can communicate with each other.&#x20;

VPC Peering can happen across AWS accounts and regions.

We can reference a security group in a peered VPC (works across accounts in the same region).

### VPC Endpoints

VPC Endpoints are powered by AWS PrivateLink, allow you to connect to AWS service using a private network instead of using internet.

VPC Endpoints are redundant & scale horizontally.

They remove the need if IGW, NATGW to access AWS service.

Common VPC issues:

* DNS setting resolution of VPC
* Route tables

#### Types of VPC Endpoints:

#### Interface Endpoints

* Powered by PrivateLink
* Provisions a ENI (private IP) as an entry point. Security groups must be attached to this ENI.
* Costs per hour & per GB of data processed.
* Interface Endpoint is preferred when access is required from on-premise VPN/Direct connect, a different VPC or region.

#### Gateway Endpoints

* Provisions a gateway & must be used as a target in a route table. (Does not use security group)
* Supports only **S3 & DynamoDB.**
* It's a free service

### VPC Flow Logs

Capture information about IP traffic going into VPCs, Subnets & ENI

Flow logs can be used to monitor & troubleshoot connectivity issues

Flow logs data can go to S3 / Cloudwatch logs

Captures information from AWS managed interfaces like ELB, RDS, ElastiCache, Redshift, Workspaces, Transit Gateway

Logs can be queries using Athena(For data stored in S3) or CloudWatch Logs insights

{% hint style="info" %}
**Incoming Requests:**

Inbound Reject =>NACL or SG

Inbound Accept, Outbound Reject => NACL (Because SG is stateful)

**Outgoing Requests:**

Outbound Reject => NACL OR SG

Outbound Accept, Inbound Reject => NACL
{% endhint %}

### Site to Site VPN

A Site to Site VPN is used to connect on premise data center to AWS

* On-prem Data Center will have a **Customer Gateway**
* VPC will have a **VPN Gateway**
* A private **site to site VPN** will be connected through the public internet

Site to Site VPN has the following pre-requisits:&#x20;

#### Virtual Private Gateway (VGW)

* VPN concentrator on the AWS Side
* VGW is created and connected to the VPC from which we want to create the site-to-site VPN Connection

#### Customer Gateway (CGW)

* Software application or physical device on the customer side of the VPN Connection
* If  the CGW has a public IP it can be used for connection
* If the CGW is behind a NAT device enabled for **NAT-T**, public IP of NAT device can be used for connection

{% hint style="info" %}
**Route Propagation** for Virtual Private Gateway (VGW) must eb enabled the route table associated with the subnet.&#x20;
{% endhint %}

To enable ping on EC2 instances, from on-premise, inbound ICMP must be enabled on the security group.

#### AWS VPN CloudHub

Provide secure communication between multiple sites, when multiple VPN connections are present

Low cost hub & spoke model for primary or secondary network connectivity between multiple locations (Only using VPN)

All traffic goes over the public internet

To set it up connect multiple VPN connections on the same VGW, setup dynamic routin and configure route tables.

### AWS Direct Connect  (DX)

Provides a dedicated **private** connection from a remote network to a VPC

Dedicated connection must be setup between on-prem DC & AWS Direct Connection locations.

A [Virtual Private Gateway](https://notes.maheshrjl.com/aws/vpc#virtual-private-gateway-vgw) is required on the VPC

Access public resource (S3) & private resources (EC2) on the same connection

Use cases:&#x20;

* Increased throughput, lower cost as connection don't go through internet
* More consistent network experience
* Hybrid Environment support
* Support IPv4 & IPv6

{% hint style="info" %}
Lead times for Direct Connect are often longer than 1 month to establish a new connection.
{% endhint %}

Data in transit through direct connect is not encrypted by default.&#x20;

AWS Direct Connect + VPN provides an IPsec encrypted connection

#### Resiliency for Direct Connect:&#x20;

#### High Resiliency for critical workloads:&#x20;

* Multiple Direct Connect are setup in multiple data centres in case 1 direct connect location is down&#x20;

#### Maximum Resiliency for critical workloads:

* We have 2 direct connect locations but, each direct connect location will have 2 independent connections.

{% hint style="info" %}
**Maxium Resilience** is achieved by separate connections terminating on seperate devices in more than one location.
{% endhint %}

> In case Direct Connect fails, we can set up a backup Direct Connect connection (expensive), or a [Site-to-Site VPN](https://notes.maheshrjl.com/aws/vpc#site-to-site-vpn) connection(Routed through public internet in case of Direct Connect Failure).&#x20;

### Transit Gateway

For having transitive  peering between thousands of VPCs & on-premises. (Hub & Spoke or STAR connection) (Without VPC peering)

Regional resources, works across regions

Share cross-account using Resource Access Manager (RAM)

Transit Gateways can be peered across region

Route tables should be created to limit which VPC can talk with other VPC

Works with Direct Connect Gateway, VPN Connections

Support **IP Multicast** (Not supported by any other AWS service)

{% hint style="info" %}
Transit Gateway can be used to increase the bandwidth of Site to Site VPN connections using **Equal Cost multi-path routing** (**ECMP**)

* Routing strategy to allow to forawrd a packet over multiple best path
{% endhint %}

> Transit Gateway can be used to share Direct Connect between multiple accounts.

### VPC Traffic Mirroring

Allows to capture & inspect network traffic in a VPC&#x20;

Traffic can be routed to security appliances managed by customer

Traffic is capture from source ENI & sent to an ENI or Network Load Balancer

Selective packet capture can be enabled

Source & Target can the same VPC or different VPC (VPC Peering)

Use case: Content inspection, thread monitoring & troubleshooting.

### Egress only Internet Gateway

Used for IPv6 only (Similar to [NAT Gateway](https://notes.maheshrjl.com/aws/vpc#nat-gateway) but for IPv6).

Allow instances in a VPC for outbound connections over IPv6 while preventing the internet to initiate an IPv6 connection the the instance.&#x20;

Route tables must be updated.&#x20;

### Networking Cost in AWS

* Incoming traffic into EC2 instances is free
* If EC2 instances are using private IP to communicate within a VPC, this is free
* If EC2 instances want to communicate across AZs either a Public IP ($0.02 per GB) or ($0.01 per GB) if using private IP.
* Inter region traffic is charged at $0.02 per GB

Use same AZ for maximum savings (at the cost high availablility)

Use private IP instead of Public IP for good savings & better performance

Ingress traffic is typically free

For Direct Connect choosing locations that are co-located in the same AWS region results lower cost for egress network.&#x20;

#### S3 Data Transfer Price

Ingress is free

S3 to internet $0.09 per GB

S3 to cloudfront is free

Cloudfront to Internet $0.085 per GB (slightly cheaper than S3)

S3 cross region replication $0.02 per GB

### AWS Network Firewall

Protect your entire VPC from layer 3 - 7

Internally AWS Network Firewall uses the AWS Gateway Load Balancer

Rules can be centraly managed cross account by AWS Firewall Manager

Active flow inspection to protect against network threats with  intrusion prevention capabilities (like Gateway Load Balancer, but managed by AWS)
