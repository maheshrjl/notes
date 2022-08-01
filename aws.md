# 🌐 AWS

## AWS CLI Commands

### EC2

`aws ec2 describe-instances` - List all ec2 instances

### IAM

List all iam users - `aws iam list-users`

### S3

Delete s3 bucket with all of it's contents :

`aws s3 rb s3://bucket-name –force`

Copy file from local machine to s3

`aws s3 cp MyFolder s3://bucket-name — recursive [–region us-west-2]`

List <mark style="color:purple;">size</mark> & <mark style="color:purple;">contents</mark> of s3 buckets - `aws s3api list-objects --bucket BUCKETNAME --output json --query "[sum(Contents[].Size), length(Contents[])]"`

## AWS IAM

### IAM user group

Groups let us specify permission for multiple users. `Identity based policies` can be attached to a group

* IAM user group is a collection of IAM users
* User groups can't be nested

### IAM Roles

Used when AWS services need to perform action on users behalf. Permissions to AWS Services are assigned with IAM Roles. Policies are attached to one principal, however, Roles can be asssumed by anyone. .

Roles can be used by:

* IAM user in the same AWS account as the role
* IAM user in a different AWS account
* A web service offered by AWS like EC2
* An external user authenticated by an external identity provider

Example Roles:

* EC2 instance roles
* Lambda function roles
* Cloudformation roles

### IAM Policy

Policies are used to manage access in AWS. Policies can be attached to IAM Identities (User, Group, or roles). Policies are evaluated when an IAM Principlpe (user / role) makes a request.

Policy Types:

* **Identitiy based policies**: Attached to IAM identities (User/Group/Roles)

<details>

<summary>Managed Policies</summary>

* AWS Managed Policies: Created & managed by AWS
* Customer managed policies: Created and managed by AWS account

</details>

<details>

<summary>Inline Policies</summary>

Policies that are added to single user, group or role. Maintain a strict one-to-one relationship between a policy & entity. They are deleted when you delete the idenitity.&#x20;

</details>

* **Resource based policies**: Attached to resources
* Permission boundaries: Defines maximum permission that identity based policies can grant to an IAM Entity (User / Role)
* Organizations SCP:&#x20;
* Access Control List (ACL):
* Session Policies:

### IAM Security Tools

#### IAM Credential Report (Account Level):

* A report that lists all account users & status of their credentials

#### IAM  Access Advisor (user-level):

* Shows service permissions granted to a user and when those services were last accessed

## EC2

### <mark style="color:yellow;">AMI</mark>

An Amazon Machine Image (AMI) is a supported and maintained image provided by AWS that provides the information required to launch an instance.

* One or more Amazon Elastic Block Store (Amazon EBS) snapshots, or, for instance-store-backed AMIs, a template for the root volume of the instance (for example, an operating system, an application server, and applications).
* Launch permissions that control which AWS accounts can use the AMI to launch instances.
* A block device mapping that specifies the volumes to attach to the instance when it's launched.

### <mark style="color:blue;">Instance Types</mark>

* M : General Purpose i.e. app server
* T :  Micro instances - Low cost, general purpose, web servers
* C, D, I : CPU/IOPS Optimised, good for memory intensive compute
* R, X : Memory Optimised
* G, P : GPU
* F: FGPA - Field programmable gate array, for hardware accleration of code&#x20;

### <mark style="color:orange;">Instance Options</mark>

#### On Demand Instance:

* Pay a fixed rate per hour. Good for apps where compute needs scaling up/down

#### Spot Instance:

* Can be extremely cheap
* Can be terminated by AWS any time
* Best for jobs that can be terminated at any time eg: batch processing
* Not charged for partial hour if terminated by AWS but, charged for full hour if user terminated

<details>

<summary>Spot Instances &#x26; Spot Fleet</summary>

#### EC2 Spot Instance Request:

* Can get a discount of 90% compared to on-demand
* Define a max spot price & get the instance while current spot price  < max

#### Spt Instance Request Methods

* [Spot Instance requests](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-requests.html#using-spot-instances-request)
* [Spot Fleet requests](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-fleet-requests.html#create-spot-fleet)
* [EC2 Fleet requests](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/manage-ec2-fleet.html#create-ec2-fleet)

</details>

#### Reserved instance:

* Fixed compute, resverved for a certain period of time. Cheaper than on-demand if used for predicatable long term. (1 or 3 years)

#### Dedicated Host:&#x20;

* Physical EC2 server are available only to customer. Used when regulatory requirements specify that you must not be using multi-tenant computing.

#### Dedicated instance:

* EC2 instances running on hardware dedicated to a single customer
* Hardware can be shared with other instances in the same account
* No control over instance placement

### <mark style="color:purple;">Security Group</mark>

* Control how traffic is allowed into or out of ec2 instances
* Security groups contain allow rules only
* Rules can be referenced by IP or by security group
* Can be attached to multiple instances
* Locked down to a single region/vpc
* All inbound traffic is blocked & outbound traffic is allowed by default
* They control:
  * Access to Ports
  * Authrozied IPv4 & IPv6 address ranges
  * Control of inbound network (from outside to the instance)
  * Control of outbound network (from the instance to outside)

{% hint style="info" %}
If a page is continously loading (timeout) it is most likely a security group issue.
{% endhint %}

### EC2 Placement Groups

1. **Cluster Placement Groups**: A logical grouping of instances within a single AZ.
   * Recommended for low network latency, and/or high network throughput applications.
   * Only specific to a single AZ
   * Can span peered VPCs in the same Region
2. **Partition Placement Groups**: Logical partition of instance groups such that no two partitions within a placement group share the same underlying hardware.
   * Reduces the impact of correlated hardware failures for your application
   * Mainly used to deploy large distributed and replicated workloads, such as HDFS, HBase, and Cassandra, across distinct racks.
   * Can have partitions in multiple Availability Zones in the same Region.
   * Offer visibility into the partitions using which you can check which instance is in which partition. Topology-aware applications, such as HDFS, HBase, and Cassandra use this information to make intelligent data replication decisions for increasing data availability and durability.
3. **Spread Placement Groups**: each instance within a spread placement group will be placed in a different rack.
   * Recommended for applications that have a small number of critical instances that should be kept separate from each other.
   * Reduces the risk of simultaneous failures that might occur when instances share the same racks which is not the case in spread placement group
   * Can span multiple Availability Zones in the same Region.

### Elastic Network Interface (ENI)

An _elastic network interface_ is a logical networking component in a VPC that represents a virtual network card. ENI's are bound to specific AZ. It can include the following attributes:

* A primary private IPv4 address from the IPv4 address range of your VPC
* One or more secondary private IPv4 addresses from the IPv4 address range of your VPC
* One Elastic IP address (IPv4) per private IPv4 address
* One public IPv4 address
* One or more IPv6 addresses
* One or more security groups
* A MAC address

ENI can be moved from one instance to another (Failover)

### EC2 Hibernate

* The in-memory (RAM) state is preserved
* The instance boot is faster (The OS has not been stopped/restarted)
* The whole state of the RAM is dumped to the root EBS volume (The root EBS volume must be ecnrypted)
* Use case: In-order to keep a long running process running, save the RAM state, Services that take long time to initialize.
* Supported instance family: C,M,R also, the RAM size must be less than 150 GB
* Not supported for bare metal instance & Amazon linux 2 & Ubuntu & Windows
* Available only for on-demand & reserved instances
* Instance cannot be hibernated for more than 60 days

### EC2 Nitro

* Underlying platform for next-gen EC2 instances (New virtualization tech)
* Enhanced networking options, HPC & IPv6 support&#x20;
* High speed EBS volume (64K EBS IOPS) only 32K IOPS supported on non nitro&#x20;
* Better security
* Instance types:&#x20;
  * Virtualized (A1, C5, C6, D3, G4)
  * Bare Metal (a1.metal, c5, c6\[metal])

### EC2 Capacity Reservations&#x20;

Planning in advance for capacity

* Capcity reservations ensure you have capacity when its needed&#x20;
* There is planned end date for reservation and 1 or 3 year commitment is not required
* Capacity access is immediate i.e. billed as soon as it starts
* AZ, number of instances & intance type/os must be specified
* Can be combined with reserved instances for cost saving



## EC2 Storage

### Instance Store

* Instance store are hardware device  (physical connection) connected directly to Ec2 instance
* Better I/O performance compared to EBS Volume
* Instance Store are ephemeral i.e. Loose data if they are stopped
* Use case: Good for buffer/cache/temporary data
* Risk of data loss if hardware (EC2 instance) fails

### EBS Volume

* Elastic Block Store (EBS) is block level storage device attached to EC2 instance
* EBS volume persist data even after instance termination (Unless delete on termination is set)
* EBS can be mounted to 1 instance at a time
* Volumes are bound to specific (Cannot be attached to instance in another AZ)
* It's a network drive (Not a physical drive)
* Capacity & IOPS must be provisioned in advance&#x20;

#### Types of EBS Volume

Amazon EBS volume types are broken into two main categories:&#x20;

* **SSD-backed volumes** are optimized for IOPS, which are best for workloads involving frequent read/write operations with small I/O size.
* **HDD-backed volumes** are optimized for throughput (measured in MiB/s) for large streaming workloads. Cannot include boot volumes.

Within each of those groups are two options. The default type is General Purpose SSD (gp2), and there are 3 others available:

* **General Purpose SSD (gp2) – general purpose, balances price and performance.**
  * **Use cases:** Most workloads such as virtual desktops, dev and test environments, and low-latency interactive apps.
* **Provisioned IOPS SSD (io1)** – highest-performance SSD volume for mission-critical low-latency or high-throughput workloads that require sustained IOPS performance, or more than 16,000 IOPS or 250 MiB/s of throughout per volume.
  * **Use cases:** Mission-critical applications, large database workloads such as MongoDB, Microsoft SQL Server, Cassandra, Oracle, MySQL, and PostgreSQL
* **Throughput Optimized HDD (st1)** – low-cost HDD volume for frequently accessed workloads with high throughput.
  * **Use cases:** Streaming workloads, big data, data warehouses, log processing.
* **Cold HDD (sc1)** – _lowest_ cost HDD volume for less-frequently accessed workloads
  * **Use cases:** Throughput-oriented storage for large volumes of data that is infrequently accessed

{% hint style="info" %}
Only gp2/gp3 & io1/io2 can be used as boot volumes
{% endhint %}

### EBS Snapshot

* Creating a backup (snapshot) of EBS volume
* Not necessary to detach volume but recommended
* Snapshot can by copied across AZ & Region
