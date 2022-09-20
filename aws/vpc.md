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

### Internet Gateway

Allows resources in a VPC to connect to the internet. It must be created seperately from a VPC.&#x20;

One VPC can only be attached to One IGW & vice versa.&#x20;

{% hint style="info" %}
Internet gateway's do not allow internet access on their own. [Route tables](https://app.gitbook.com/s/d8qGZNNEP3vhnf91dxDT/\~/changes/fq73XzQhOre23mAFHreJ/aws/vpc#route-table) must be configured for internet access.
{% endhint %}

### Route Table

Define & configure traffic route to & from the subnet.&#x20;

Public route tables are mapped to public route tables and private route tables are mapped to private subnets.

Routes must be configred for Route tables. Eg: For a public route table any traffic that does not match the subnet (local ip) should be going to the IGW.&#x20;

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

AWS managed NAT, it has high badnwidth and high availability.&#x20;

Pay per hour for usage and bandwidth. Supports upto 45 Gbps bandwidth.&#x20;

NATGW is created in a specifc AZ, it uses a elastic IP.&#x20;

Can't be used by a EC2 instance in the same subnet.&#x20;

NAT Gateway cannot be used as a bastion host.&#x20;

Routes must be edited in the route table to allow traffic to the internet gateway.

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