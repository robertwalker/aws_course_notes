Virtual Private Cloud (VPC) Overview
------------------------------------

Amazon Virtual Private Cloud (Amazon VPC) lets you provision a logically isolated section of the AWS Cloud where you can launch AWS resoruces in a virtual network that you define. you have complete control over your virtual networking environment.

#### Includes

* Selection of your own IP address range
* Creation of subnets
* Configuration of route tables and network gateways

You can easily customize the network configuration for your Amazon Virtual Private Cloud (VPC).

Example: Create a public-facing subnet for your web servers that has access to the Internet, and place your backend systems, such as databases or application servers in a private subnet with no Internet access.

You can leverage multiple layers of security, including security groups and network access control lists, to help control access to Amazon EC2 instances in each subnet.

### Exam Tips

* Security Groups can span subnets
* 1 subnet, 1 Availability Zone (AZ)
* 1 Internet Gateway per VPC
* Internet Gateways are Global

#### Private IP ranges

* 10.0.0.0 - 10.255.255.255 (10/8 prefix)
* 172.16.0/0 - 172.31.255.255 (172.16/12 prefix)
* 192.168.0.0 - 192.168.255.255 (192.168/16 prefix)

[CIDR.xyz](http://cidr.xyz) -- An interactive IP address and CIDR range visualizer

Smallest IP prifix allowed by Amazon is 10/28 prefix (16 IPs)

### Features of a VPC

* Launch instances into a subnet of your choosing
* Assign custom IP address ranges in each subnet
* Configure route tables between subnets
* Create Internet gateway and attach it to your VPC (again 1 Internet Gateway per VPC)
* Much better security control over your AWS resources
* Instance security groups (can span AZs and subnets)
* Subnet network Access Control Lists (ACLs)

### Default VPC vs. Custom VPC

* Default VPC is provisioned by Amazon allowing you to immediately deploy instances
* All subnets in Default VPC have a route to the Internet
* You do not get private subnets in a Default VPC, must be set up
* Each EC2 instance has both a public and private IP address when deployed to a Default VPC
* Instances deployed to a Custom VPC that has a private subnet you only get a private IP address

### VPC Peering

* Allows you to connect one VPC to another via a direct network route using private IP addresses
* Instances behave as if they were on the same private network
* You can peer VPCs with other AWS accounts as well as with other VPCs in the same account
* Peering is in a star configuration (i.e. 1 central VPC peers with 4 others). NO TRANSITIVE PEERING!!!

### More Exam Tips

* Think of a VPC as a logical datacenter in AWS
* Consists of
  * IGWs (i.e. Virtual Private Internet Gateways)
  * Route Tables
  * Network Access Control Lists (ACLs)
  * Subnets
* Security Groups
* 1 Subnet = 1 Availability Zone (AZ)
* Security Groups are Stateful; Network Access Control Lists are Stateless
* NO TRANSITIVE PEERING

VPC Lab Notes
-------------

* Create VPC
  * VPC Name
  * IPv4 CIDR block: 10.0.0.0/16
  * IPv6 CIDR block:
    * No IPv6 Block
    * Amazon provided IPv6 CIDR block (selected)
  * Tenancy:
    * Default (selected)
    * Dedicated [Hardware]
* After creating a VPC
  * Shares subnets with Default VPC (uses same list)
  * Creates a new route table
  * No gateways created (leaves the Default VPC IGW)
  * Creates default network ACL
  * Creates default Security Group
* Create Subnet
  * Name Tag: 10.0.1.0 -
  * VPC: Select new VPC from list
  * VPC CIDRs: Displays based on VPC selected
  * Availability Zone: us-east1a (These are per account. Actual AZs are randomized by Amazon)
  * IPv4 CIDR block: 10.0.1.0/24
  * IPv4 CIDR block:
    * Don't assign an IPv6 CIDR (selected)
    * Specify a custom IPv6 CIDR
* After creating subnet
  * 251 IP addresses available for your use
    * 10.0.0.0 -- Network Address
    * 10.0.0.1 -- Reserved by AWS for the VPC router
    * 10.0.0.2 -- Reserved by AWS for DNS server
    * 10.0.0.3 -- Reserved by AWS for future use
    * 10.0.0.255 -- Network Broadcast Address
* Repeat Create Subnet for 10.0.2.0 -
* Create Internet Gateway
  * Name tag: MyIGW
* Attach Gateway (Detached by default)
  * Can only attach 1 IGW per VPC
* Create Route Table
  * Name tag: MyInternetRouteOut
  * VPC: Select VPC from list
* Update New Route Table
  * Select [Routes] tab and press [Add another route]
    * Destination: 0.0.0.0/0
    * Target: Select your IGW
    * Press [Save]
  * Press [Add another route] to add IPv6
    * Destination: ::/0
    * Target: Select your IGW
    * Press [Save]
  * Select [Subnet Associations] tab
    * Press [Edit]
    * Select Subnet (10.1.1.0 - us-east-1a)
    * Press [Save]
* Update Subnets
  * Select your public subnet (10.0.1.0 - us-east-1a)
  * Select [Subnet Actions] -> [Modify auto-assigned IP settings]
  * Enable auto-assign public IPv4 addresses (checked)
* Launch an EC2 Instance into your new VPC
  * Network: Select your VPC
  * Subnet: Select 10.0.1.0 - us-east-1a
  * Leave rest of details at default
  * Press [Add Storage] and Add Tags
  * Create a new Security Group (Security Groups don't span VPCs)
  * Launch EC2 Instance
* Launch an EC2 Instance into your private Subnet
  * Use defualt Security Group for this one

### Your VPC So Far (Diagram)

[VPC with Public & Private Subnets (PNG)](resources/1B6FFBAA6138DD9CD3853B5499F95B3D.png)

### Lab Notes -- Part 2

* Create new Security Group
  * Security group name: My-RDS-SG
  * Description: My-RDS-SG
  * Security group rules [Inbound]
    * See Rules Table below...
* In EC2 assign your new Security Group

#### Rules

| Type         | Protocol | Port Range | Source              |
|--------------|----------|------------|---------------------|
| SSH          | TCP      | 22         | Custom: 10.0.1.0/24 |
| MySQL/Aurora | TCP      | 3306       | Custom: 10.0.1.0/24 |
| HTTP         | TCP      | 80         | Custom: 10.0.1.0/24 |
| HTTPS        | TCP      | 443        | Custom: 10.0.1.0/24 |
| All ICMP -IPv| ICMP     | 0 - 65535  | Custom: 10.0.1.0/24 |

NAT Instances & NAT Gateways
----------------------------

### Lab Notes

#### Creating a NAT Instance

* Select EC2 form AWS managmentment console
* Click [Launch Instance] button
* Select [Community AMIs] in sidebar
* Search for `NAT`
* Find `amzn-ami-vpc-nat-hvm-2014.09.1.x86_64-gp2` and press [Select] button
* Choose a t2.micro Instance Type
* Use defaults except:
  * Network: Select your custom VPC
  * Subnet: Select your public Subnet (10.0.1.0 - us-east-1a)
* [Add Storage] --> [Add Tags]
  * Name: NAT-INSTANCE
* [Configure Security Group]
  * Select an existing security group (radio button)
  * Select `Web-DMZ`
* [Review and Launch]
* Needs HTTPS configured for Security Group
* Select [Instances] from sidebar
* Select your `NAT-INSTANCE` in the table
* [Actions] --> [Networking] --> [Change Source/Dest. Check]
* Disable `Source/Dest. Check` (Note: Do this only for NAT Instances)
* Select your VPC from the AWS management console
* Select [Route Tables] from sidebar
* Select your default Route Table for your VPC
* [Edit] and [Add another route]
  * Destintation: 0.0.0.0/0
  * Target: Select your `NAT-INSTANCE`
  * Press [Save]

#### Creating a NAT Gateway

* Select VPC from AWS management console
* Select [NAT Gateway] from sidebar
* Press [Create NAT Gateway]
  * Subnet: Select your public subnet (10.0.1.0 - us-east-1a)
  * Elastic IP Allocation ID: Press [Create New EIP]
  * Press [Create a NAT Gateway]
  * On the resulting screen there is a [Edit route tables] button, but it takes some time to launch the gateway so don't use it for this lab
  * Press [Close]
* Go back to [NAT Gateways] under VPC Dashboard
* Make sure gateway is available
* Select [Route Tables] from sidebar
* Select your default Route Table in the list
* Select [Routes] tab
* Press [Edit] --> [Add another route]
  * Destination: 0.0.0.0/0
  * Target: Select NAT Gateway (nat-[hash-giberish])
  * Press [Save]

#### Notes

* `NAT Gateway` is used for IPv4 only, `Egress Only Internet Gateway` is used for IPv6.
* Google "Comparison of NAT Instances and NAT Gateways" to find Amazon's summary page on the subject.
* NAT Gateways are preferred over NAT Instances whenever possible.

### Our VPS So Far (Diagram)

[VPC with Public & Private Subnets (PNG)](resources/05B8C5C691127DF289552459CED226AA.png)

### Exam Tips

#### NAT Instances

* When creating a NAT Instance, disable `Source/Destination Check` on the instance
* NAT Instances must be in public subnet
* There must be a route out of the private subnet to the NAT instance in order for this to work
* The amount of traffic that NAT instances can support depends on the instance size. If you are bottlenecking, increase the instance size. (Don't use t2.micro for production for example)
* You can create high availability using Autoscaling Groups, multiple subnets in different AZs, and a script to automate failover.
* Behind a Security Group

#### NAT Gateways

* Preferred by the enterprise
* Scale automatically up to 10Gbps
* No need to patch
* Not associated with Security Groups
* Automatically assigned a public IP address
* Remember to update your Route Tables
* No need to disable Source/Destination Checks

Network Access Control Lists vs. Security Groups
------------------------------------------------

*** more to come ***