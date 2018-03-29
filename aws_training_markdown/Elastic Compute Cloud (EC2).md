EC2 101
-------

### Options

* On Demand -- Pay a fixed rate by the hour (or second) with no commitment
* Reserved -- Capacity reservation on 1 or 3 year terms. Reduced cost
* Spot -- Bid on price. Price based on current demand
* Dedicated Hosts -- Physical EC2 server dedicated for your use

### On Demand

* Low cost, flexible without up-front payment or long-term commitment
* Applications with short term, spiky, or unpredictable workloads that cannot be interrupted
* Applictions being developed or tested for the first time

### Reserved

* Applications with steady state or predictable usage
* Applications that require reserved capacity
* Upfront payments reduce total computing costs even further
  * Standard RI's (Up to 75% off compared to On-Demand)
  * Convertable RI's (Up to 54% off compared to On-Demand) and can change attributes of the RI
  * Scheduled RI's available to launch withing the time window you reserve.

### Spot

* Applications that have flixible start and end times
* Applications that are only feasible at very low compute prices
* Users with urgent computing needs for large amounts of additional capacity

### Dedicated Hosts

* Useful for regulatory requirements that may not support multi-tenant virtualization
* Licenseing that does not support multi-tenancy or cloud deployments
* Can be purchased on-demand (hourly)
* Can be purchased as a reservation for up to 70% off the On-Demand price

EC2 Instance Types
------------------

Memorazation Device: Dr Mc GIFT PX (Remember the cartoon duck from lecture slide)

* D2 -- Dense Storage (Fileservers, Date Warehousing, Hadoop)
* R4 -- Memory Optimized (Memory Intensive Apps, DBs)
* M4 -- General Purpose (Application Servers)
* C4 -- Compute Optimized (CPU Intensive Apps, DBS)
* G2 -- Graphics Intensive (Video Encodeing, 3D App Streaming)
* I2 -- High Speed Storage (NoSQL DBs, Data Warehousing, etc.)
* F1 -- Field Programmable Gate Array (Hardware acceleration for code)
* T2 -- Lowest Cost, General Purpose (Web Servers, Small DBs)
* P2 -- Graphics/General Purpose GPU (Macine Learning, Crypto Currency mining, etc.)
* X1 -- Memory Optimized (SAP HANA, Apache Spark, etc.)

Elastic Block Storage (EBS)
---------------------------

### SSD

* General Purpose SSD (GP2)
  * Balances both price and performance
  * Ratio of 3 IOPS per GB, up to 10,000 IOPS, burst up to 3,000 IOPS for extended periods of time for volumns 3.334 GiB and above
* Provisioned IOPS SSD (IO1)
  * Designed for I/O intensive applications such as large RDBMS or NoSQL
  * Use if you need more than 10,000 IOPS
  * Provision up to 20,000 IOPS per volume

### HDD

* Throughput Optimized HDD (ST1)
  * Big Data
  * Data Warehouse
  * Log Processing
  * Cannot be a boot volume
* Cold HDD (SC1)
  * Lowest cost storage for infequently accessed workloads
  * File Server
  * Cannot be boot volume
* Magnetic (Standard)
  * Lowest cost per gigabyte
  * Bootable

Exam Tips
---------

### EC2 Pricing Models

* Know the differences between
  * On Demand
  * Spot
  * Reserved
  * Dedicated Hosts
* Remember with spot instances
  * If you terminate the instance, you pay for the hour
  * If AWS terminates the spot instance, you get the hour it was terminated in for free

### EBS

* Consists of
  * SSD, General Purpoee -- GP2 (Up to 10,000 IOPS)
  * SSD, Provisioned IOPS -- IO1 (Up to 10,000 IOPS)
  * HDD, Throughput Optimized -- ST1 (frequently aceessed workloads)
  * HDD, Cold -- SC1 (less frequently accessed data)
  * HDD, Magnetic -- Standard (cheap, infrequently accessed)
* Cannot mount 1 EBS volume to multiple EC2 instances, instead use EFS

### EC2 Instance Types

* Remember Dr Mc GIFT PX
  * D for Density
  * R for RAM
  * M for Main choice
  * C for Compute
  * G for Graphics
  * I for IOPS
  * F for Field Programmable Gate Array (FPGA) 
  * T for T2 Micro -- cheap general purpose
  * P for Pics -- Graphics
  * X for Extreme Memory

EC2 Labs
--------

### Step 1

Choose an Amazon Machine Image (AMI)

### Step 2

Choose an Instance Type (Dr Mc GIFT PX). Example: t2.micro

### Step 3

Configure Instance Details. Number of instances, purchasing options, network, IAM role, shutdown behavior, monitoring, etc.

Exam Tip: 1 subnet for 1 availablity zone

### Step 4

Add Storage. (SSD, HDD types)

Exam Tip: By default storage volumns are deleted on instance termination.

### Add Tags

Add metadata to instances.

### Configure Security Group

This is virtual firewall configuration settings.

### Exam Tips

* Termination Protection is turned off by default
* On an EBS-backed instance the default action is for the root EBS volume to be deleted on instance termination
* EBS Root Volumes of your DEFAULT AMIs cannot by encrypted. It is possible to use third-party tools (e.g. Bit Locker) to encrypt the root volume. Or, this can be done when creating AMIs in the AWS console or using the API.
* Additional volumes can be encrypted

Security Groups
---------------

Virtual firewalls.

* Changing FW rules applies immediately
* Security Groups are stateful. Adding inbound rule adds matching outbound rule.
* Network Access Control Lists are stateless. (These are separate from Security Groups)
* Deny by default. Add rules to allow.
* Cannot block specific IP addressses using Security Groups, instead use Netowrk Access Control Lists

Upgrading EBS Volumes
---------------------

EBS volumes must be in the same availability zone as instance. Volumes can be modified to change their type and size (Note: Standard magnetic volumes cannot be modified).

### Exam Tips

* Volumes exist on EBS
* Snapshots exist on S3
* Snapshots are point-in-time copies of volumes
* Snapshots are incremental
* First snapshots may take some time
* Stop instance to create EBS volumes that serve as root devices and to take snapshots
* It is possible to take snapshots while the instance is running
* You can create AMIs from both Volumes and Snapshots
* You can change EBS volume size on the fly, including changing size and storage type
* Volumes will ALWAYS be in the same availabilty zone as EC2 instance
* To move EC2 volume from one AZ/Region to another, take a snapshot or an image of it, then copy it to the new AZ/Region.
* Snapshots of encrypted volumes are encrypted
* Volumes restored from encrypted snapshots are encrypted
* You can share shapshots with other AWS accounts or publicly, but only if they are unencrypted

RAID Volumes and Snapshots
--------------------------

* Redundant Array of Independent Disks
  * RAID 0 -- Striped, No Redundancy, Good Performance
  * RAID 1 -- Mirrored, Redundancy
  * RAID 5 -- Good for reads, bad for writes, AWS does not recommend on EBS
  * RAID 10 -- Striped and Mirrored, Good Redundancy, Good Performance
  
Taking snapshots of instance with RAID requires an application consistent snapshot.

* Freeze the file system
* Unmount the RAID array
* Shut down the associated EC2 instance

Encrypted Root Device Volume
----------------------------

1. Shutdown the instance
2. Create a snapshot
3. Copy snapshot and turn on encryption (can be copied to another AZ/Region)

AMI Types
---------

All AMIs are categorized as either backed by Amazon EBS or backed by instance store.

For EBS Volumes: The root device for an instance launched from AMI is an Amazon EBS volume created from Amazon EBS snapshot.

For Instance Store Volumes: The root device for an instance launched form the AMI is an instance store volume created from a template stored in Amazon S3.

Elastic Load Balancers (ELB)
----------------------------

* Application Load Balancer (Layer 7)
* Classic Load Balancer (Layer 4)

Exam questions currently cover Classic Load Balancers. Application Load Balancer is relatively new to AWS.

### Exam Tips

* Instances monitored by ELB are reported as InService or OutOfService
* Health checks check instances health by talking to them
* Have their own DNS name. You're never given an IP address
* Read the ELB FAQ for Classic Load Balancers

CloudWatch
-----------

CloudWatch is under Managment Tools in the AWS Dashboard.

There are 4 types of widgets available. Use Markdown to creating metrics components.

### Exam Tips

* Dashboard -- Create dashboards to monitor your AWS environment
* Alarms -- Set alarms that notify when configured threadholds are hit
* Events -- Respond to state changes in your AWS resources
* Logs -- Aggregate, monitor and store log files

AWS Command Line Interface (CLI)
--------------------------------

AWS CLI is automaitcally installed on Amazon Linux AMI.

Identy Access Management Roles
------------------------------

Roles are Global. Using roles allows access from CLI/API without storing credentials locally.

Metadata
--------

SSH into instance and use `curl` to read metadata. It's important to remember this URL for the exam.

    curl http://169.254.169.254/latest/meta-data/

Read public IPv4 address:

    curl http://169.254.169.254/latest/meta-data/public-ipv4

Launch Configurations and Auto Scaling Groups
---------------------------------------------

In order to create Auto Scaling Groups you must first create a Launch Configuration.

### Launch Configrations

* Simalar process as creating EC2 instances.
* Bash scripts can be entered in "Advanced Details" under "User data" as text or file. Can be base64 encoded if checkbox is selected.

Placement Groups
----------------

Logical grouping of instances within an Availability Zone. Enables applications to participate in low latency, 10 Gbps network. Recommended for applicaitons that benefit from low latency, high network throughput, or both. (Hadoop clusters, etc.)

* Can't span multiple Availability Zones
* Name must be unique within your AWS account
* Instance types are limited (Compute Optimized, GPU, Memory Optimized, Storage Optimized)
* Homogenous instances are recommended (Same family and size)
* Can't be merged
* Existing instances can't be moved into a placement group. You can create an AMI from your existing instance and launch a new instance from the AMI.

Elastic File System (EFS)
-------------------------

File storage service that can grow or shrink automatically as you add and remove files.

* Supports Network File System version 4 (NFSv4) protocol
* Only pay for the storage you use
* Can scale to petabyte scale
* Can support thousands of concurrent NFS connections
* Data is stored across multiple AZs within a region
* Read after write consistence
* Block based storage

Lambda Concepts
---------------

A compute service where you can upload code functions. Lamda takes care of provisioning and managing the servers that run the code. No operating systems, patching, scaling, etc. to worrry about.

* As an event-driven service (e.g. S3 or DynamoDB events)
* As a compute service in response to HTTP requests via API Gateway or AWS SDK.

### Triggers

*  API Gateway
*  AWS IoT
*  Alexa Skills Kit
*  Alexa Smart Home
*  CloudFront
*  CloudWatch Events
*  CloudWatch Logs
*  CodeCommit
*  Cognito Sync Trigger
*  DynamoDB
*  Kinesis
*  S3
*  SNS

### Languages

*  Node.js
*  Java
*  Python
*  C#

### Pricing

* Number of requests
  * First 1 million requests are free. $0.20 per 1 million requests
* Duration
  * Duration is calculated from the time your code begins executing until it returns or otherwise terminates, rounded up to th enearest 100ms. Price depends on the amount of memory you allocate to your function at a rate of $0.00001667/GB-second used.
* 5 minute threashold per function
* No servers
* Continuous scaling
* Super cheap

Amazon Echo is an example use case for Lambda.

### Exam Tips

* Scales out (not up) automatically
* Functions are independent, 1 event = 1 function
* Serverless
* Know what services are serverless!
* Functions can trigger other functions, 1 event can = n fuctions if functions trigger functions
* Architectures can get extremely complicated, ASW X-ray helps with debugging
* Can do things globally, can use it to back up S3 buckets to other S3 buckets, etc.
* Know your triggers!
* 5 minute max execution time

EC2 Summary and More Exam Tips
------------------------------

* READ THE EC2 FAQ
* Know the difference between:
  * On Demand
  * Spot
  * Reserved
  * Dedicated Hosts
* Spot Instances
  * If you terminate the instance, you pay for the hour
  * If AWS terminates the instance, you get the hour for free
* Dr Mc GIFT PX
  * Refer to *EC2 Instance Types* above in these notes
* EBS consists of:
  * SSD -- General Purpose -- GP2 (Up to 10,000 IOPS)
  * SSD -- Provistioned IOPS -- IO1 (More than 10,000 IOPS)
  * HDD -- Throughput Optimized -- ST1 -- frequently accessed workloads
  * HDD -- Cold -- SC1 -- less frequently accessed data
  * HDD -- Magnetic -- Standard -- cheap, infrenquently accessed data
* You cannot mount one EBS volume to multiple EC2 instances, instead use EFS
* Termination Protection is off by default
* Root EBS volume is deleted on instnace termaination by default
* Root Volumes cannot be encrypted by default, you need third-party tools to encrypt
* Addtional Volumes can be encrypted
* Volumes exist in EBS -- Virtual Hard Disk
* Snapshots
  * Stored in S3
  * Point-in-time copies of Volumes
  * Are incremental, only changes since last snapshot are stored
  * First snapshot can take some time to create
  * Encrypted volumes are encrypted automatically
  * Volumes restored from encrypted snapshots remain encrypted
  * Can be shared, but only if unencrypted
  * Can be shared between AWS accounts or made public
  * Stop the instance before taking snapshots of a root volume
* EBS vs Instance Store
  * Sometimes called Ephemeral Storage
  * Cannot be stopped, If the underlying host fails all data is lost
  * EBS backed instances can be stopped, data will persist
  * Either type can be rebooted without losing data
  * By default root volumes are deleted on termination, but EBS can be set to keep the root volume
* Snapshots of RAID arrays
  * Problem -- Snapshots lose data in RAID caches
  * Solution -- Take an application consistent snapshot
    * Freeze the file system
    * Unmount the RAID array
    * Shut down the associated EC2 instance
* Amazon Macine Images
  * AMIs are regional -- Can only launch from the region where stored. Can be copied to other regions with CLI or API.
* Standard Monitoring = 5 mins
* Detailed Monitoring = 1 min
* CloudWatch is for performace monitoring
* CloudTrail is for auditing
* CloudWatch
  * Dashboards
  * Alarms
  * Events
  * Logs
* Roles
  * More secure than storing access keys and secrets
  * Easier to manage
  * Can be assigned to instance after it has been provisioned via CLI or AWS console
  * Global
* Instance Meta-data
  * Get information about instances such as IP address
  * curl `http://169.254.169.254/latest/meta-data/`
* Elastic File System (EFS) -- Probably not on exam currently
  * NFSv4
  * Pay only for storage you use (no provisioning)
  * Scales up to petabytes
  * Supports thousands of concurrent NFS connections
  * Data is stored across multiple AZs within a region
  * Read after write consistency
* Lambda
  * Serverless compute, event driven or response to HTTP via API Gateway or API calls