# AWS Virtual Private Cloud - VPC

* A virtual private cloud (VPC) is a virtual network dedicated to the AWS account. It is logically isolated from other virtual networks in the AWS cloud.
* VPC allows the user to select IP address range, create subnets, and configure route tables, network gateways, and security settings.
* **VPC Sizing**
    * VPC needs a set of IP addresses in the form of a Classless Inter-Domain Routing (CIDR) block_ for e.g, 10.0.0.0/16, which allows 2^16 (65536) IP address to be available _
    * Allowed CIDR block size is between 
        * /28 netmask (minimum with 2^4 – 16 available IP address) and
        * /16 netmask (maximum with 2^16 – 65536 IP address)
    * CIDR block from private (non-publicly routable) IP address can be assigned 
        * 10.0.0.0 – 10.255.255.255 (10/8 prefix)
        * 172.16.0.0 – 172.31.255.255 (172.16/12 prefix)
        * 192.168.0.0 – 192.168.255.255 (192.168/16 prefix)
    * It's possible to specify a range of publicly routable IP addresses; however, direct access to the Internet is not currently supported from publicly routable CIDR blocks in a VPC
    * CIDR block once assigned to the VPC cannot be modified.  **NOTE** – You can now resize VPC. Read [AWS blog post.][5]
    * Each VPC is separate from any other VPC created with the same CIDR block even if it resides within the same AWS account
* VPC allows [VPC Peering][6] connections with other VPC within the same or different AWS accounts
* Connection between your VPC and corporate or home network can be established, however the CIDR blocks should be not be overlapping _for e.g. VPC with CIDR 10.0.0.0/16 can communicate with 10.1.0.0/16 corporate network but the connections would be dropped if it tries to connect to 10.0.37.0/16 corporate network cause of overlapping ip addresses_.
* VPC allows you to set tenancy option for the Instances launched in it. By default, the tenancy option is shared. If dedicated option selected, all the instances within it are launched on a dedicated hardware overriding the individual instance tenancy setting
* Deletion of the VPC is possible only after terminating all instances within the VPC, and deleting all the components with the VPC _for e.g. subnets, security groups, network ACLs, route tables, Internet gateways, VPC peering connections, and DHCP options_

![AWS VPC Components][7]
## IP Addresses

Instances launched in the VPC can have Private, Public and Elastic IP address assigned to it and are properties of ENI (Network Interfaces)

* Private IP Addresses 
    * Private IP addresses are not reachable over the Internet, and can be used for communication only between the instances within the VPC
    * All instances are assigned a private IP address, within the IP address range of the subnet, to the default network interface
    * Primary IP address is associated with the network interface for its lifetime, even when the instance is stopped and restarted and is released only when the instance is terminated
    * Additional Private IP addresses, known as secondary private IP address, can be assigned to the instances and these can be reassigned from one network interface to another
* Public IP address 
    * Public IP addresses are reachable over the Internet, and can be used for communication between instances and the Internet, or with other AWS services that have public endpoints
    * Public IP address assignment to the Instance depends if the Public IP Addressing is enabled for the Subnet.
    * Public IP address can also be assigned to the Instance by enabling the Public IP addressing during the creation of the instance, which overrides the subnet's public IP addressing attribute
    * Public IP address is assigned from AWS pool of IP addresses and it is not associated with the AWS account and hence is released when the instance is stopped and restarted or terminated.
* Elastic IP address 
    * Elastic IP addresses are static, persistent public IP addresses which can be associated and disassociated with the instance, as required
    * Elastic IP address is allocated at an VPC and owned by the account unless released
    * A Network Interface can be assigned either a Public IP or an Elastic IP. If you assign an instance, already having an Public IP, an Elastic IP, the public IP is released
    * Elastic IP addresses can be moved from one instance to another, which can be within the same or different VPC within the same account
    * Elastic IP are charged for non usage i.e. if it is not associated or associated with a stopped instance or an unattached Network Interface

## Elastic Network Interface (ENI)

* Each Instance is attached with default elastic network interface (Primary Network Interface eth0) and cannot be detached from the instance
* ENI can include the following attributes 
    * Primary private IP address
    * One or more secondary private IP addresses
    * One Elastic IP address per private IP address
    * One public IP address, which can be auto-assigned to the network interface for eth0 when you launch an instance, but only when you create a network interface for eth0 instead of using an existing ENI
    * One or more security groups
    * A MAC address
    * A source/destination check flag
    * A description
* ENI's attributes follow the ENI as it is attached or detached from an instance and reattached to another instance. When an ENI is moved from one instance to another, network traffic is redirected to the new instance.
* Multiple ENIs can be attached to an instance and is useful for use cases: 
    * Create a management network.
    * Use network and security appliances in your VPC.
    * Create dual-homed instances with workloads/roles on distinct subnets.
    * Create a low-budget, high-availability solution.

## Route Tables

* Route table defines rules, termed as routes, which determine where network traffic from the subnet would be routed
* Each VPC has a implicit router to route network traffic
* Each VPC has a Main Route table, and can have multiple custom route tables created
* Each Subnet within a VPC must be associated with a single route table at a time, while a route table can have multiple subnets associated with it
* Subnet, if not explicitly associated to a route table, is implicitly associated with the main route table
* Every route table contains a local route that enables communication within a VPC which cannot be modified or deleted
* Route priority is decided by matching the most specific route in the route table that matches the traffic
* Route tables needs to be updated to defined routes for Internet gateways, Virtual Private gateways, VPC Peering, VPC Endpoints, NAT Device etc.

## Internet Gateways – IGW

* An Internet gateway is a horizontally scaled, redundant, and highly available VPC component that allows communication between instances in the VPC and the Internet.
* IGW imposes no availability risks or bandwidth constraints on the network traffic.
* An Internet gateway serves two purposes: 
    * To provide a target in the VPC route tables for Internet-routable traffic,
    * To perform network address translation (NAT) for instances that have been assigned public IP addresses.
* **Enabling Internet access to an Instance requires**
    * Attaching Internet gateway to the VPC
    * Subnet should have route tables associated with the route pointing to the Internet gateway
    * Instances should have a Public IP or Elastic IP address assigned
    * Security groups and NACLs associated with the Instance should allow relevant traffic

## NAT

NAT device enables instances in a private subnet to connect to the Internet or other AWS services, but prevents the Internet from initiating connections with the instances.



# AWS VPC NAT - NAT Gateway

* Network Address Translation (NAT) devices, launched in the public subnet, enables instances in a private subnet to connect to the Internet, but prevents the Internet from initiating connections with the instances.
* Instances in private subnets would need internet connection for performing software updates or trying to access external services
* NAT device performs the function of both address translation and port address translation (PAT)
* NAT instance prevents instances to be directly exposed to the Internet and having to be launched in Public subnet and assignment of the Elastic IP address to all, which are limited.
* NAT device routes the traffic, from the private subnet to the Internet, by replacing the source IP address with its address and for the response traffic it translates the address back to the instances' private IP addresses.
* AWS allows NAT configuration in 2 ways 
    * NAT Instance
    * NAT Gateway, managed service by AWS

## NAT device Configuration Key Points

* needs to be launched in the Public Subnet
* needs to be associated with an Elastic IP address (or public IP address)
* should have the **Source/Destination flag disabled** to route traffic from the instances in the private subnet to the Internet and send the response back
* should have a Security group associated that 
    * allows Outbound Internet traffic from instances in the private subnet
    * disallows Inbound Internet traffic from everywhere
* Instances in the private subnet should have the Route table configured to direct all Internet traffic to the NAT device

## NAT Gateway

NAT gateway is a AWS managed NAT service that provides better availability, higher bandwidth, and requires less administrative effort.

* A NAT gateway supports bursts of up to 10 Gbps of bandwidth.
* For more than 10 Gbps bursts requirement, the workload can be distributed by splitting the resources into multiple subnets, and creating a NAT gateway in each subnet.
* NAT gateway is associated with One Elastic IP address which cannot be disassociated after it's creation.
* Each NAT gateway is created in a specific Availability Zone and implemented with redundancy in that zone.
* A NAT gateway supports the following protocols: TCP, UDP, and ICMP.
* NAT gateway cannot be associated a security group. Security can be configured for the instances in the private subnets to control the traffic
* Network ACL can be used to control the traffic to and from the subnet. Network ACL applies to the NAT gateway's traffic, which uses ports 1024 – 65535.
* NAT gateway when created receives an elastic network interface that's automatically assigned a private IP address from the IP address range of your subnet. Attributes of this network interface cannot be modified
* NAT gateway cannot send traffic over VPC endpoints, VPN connections, AWS Direct Connect, or VPC peering connections. Private subnet's route table should be modified to route the traffic directly to these devices.

## NAT Instance

* NAT instance can be created by using Amazon Linux AMIs configured to route traffic to Internet.
* They do not provide the same availability and bandwidth and need to configured as per the application needs.
* NAT instances must have security groups associated with Inbound traffic enabled from private subnets and Outbound traffic enabled to the Internet
* NAT instances should have the Source Destination Check attribute disabled, as it is neither the source nor the destination for the traffic and merely acts as a gateway


* Create One NAT instance per Availability Zone
* Configure all Private subnet route tables to the same zone NAT instance
* User Auto Scaling for NAT availability
* User Auto Scaling group per NAT instance with min and max size set of 1. So if NAT instances fail, Auto Scaling will automatically launch an replacement instance
* NAT instance is highly available with limited downtime
* Let Auto Scaling monitor health and availability of the NAT instance
* Bootstrap scripts with the NAT instance to update the Route tables programmatically
* Keep a close watch on the Network Metrics and scale vertically the NAT instance type to the one with high network performance

### Disabling Source/Destination checks

* Each EC2 instance performs source/destination checks, by default, and the instance must be the source or destination of any traffic it sends or receives.
* However, as the NAT instance acts as a router between the Internet and the instances in the private subnet it must be able to send and receive traffic when the source or destination is not itself.
* **Therefore, the source/destination checks on the NAT instance should be disabled**


## VPC Security

Security within a VPC is provided through

* Security groups – Act as a firewall for associated EC2 instances, controlling both inbound and outbound traffic at the instance level
* Network access control lists (ACLs) – Act as a firewall for associated subnets, controlling both inbound and outbound traffic at the subnet level
* Flow logs – Capture information about the IP traffic going to and from network interfaces in your VPC

### Security Groups & NACLs


# AWS VPC Security - Security Group vs NACLs

* In a VPC, both Security Groups and Network ACLs (NACLS) together help to build a layered network defense.
* Security groups – Act as a firewall for associated Amazon instances, controlling both inbound and outbound traffic at the instance level
* Network access control lists (NACLs) – Act as a firewall for associated subnets, controlling both inbound and outbound traffic at the subnet level



## Security Groups

* Acts at an Instance level and not at the subnet level.
* Each instance within a subnet can be assigned a different set of Security groups
* An instance can be assigned 5 security groups with each security group having 50 rules
* Security groups allows you to add or remove rules (authorizing or revoking access) for both Inbound (ingress) and Outbound (egress) traffic to the instance 
    * Default Security group allows no external inbound traffic but allows inbound traffic from instances with the same security group
    * Default Security group allows all outbound traffic
    * New Security groups start with only an outbound rule that allows all traffic to leave the instances
* Security groups can **specify only Allow rules, but not deny rules**
* Security groups can grant access to a specific CIDR range, or to another security group in the VPC or in a peer VPC (requires a VPC peering connection)
* Security groups are evaluated as a Whole or Cumulative bunch of rules with the most permissive rule taking precedence. _For e.g. if you have a rule that allows access to TCP port 22 (SSH) from IP address 203.0.113.1 and another rule that allows access to TCP port 22 from everyone, everyone has access to TCP port 22._
* Security groups are **Stateful – **responses to allowed inbound traffic are allowed to flow outbound regardless of outbound rules, and vice versa. Hence an Outbound rule for the response is not needed
* Security groups are associated with ENI (network interfaces).
* Security groups associated with the instance can be changed, which changes the security groups associated with the primary network interface (eth0) and the changes would be applicable immediately to all the instances associated with the Security group

### Connection Tracking

* As Security groups are Stateful and they use Connection tracking to track information about traffic to and from the instance.
* Responses to inbound traffic are allowed to flow out of the instance regardless of outbound security group rules, and vice versa.
* Connection Tracking is maintained only if there is no explicit Outbound rule for an Inbound request (and vice versa)
* However, if there is an explicit Outbound rule for an Inbound request, the response traffic is allowed on the basis of the Outbound rule and not on the Tracking information
* Tracking flow e.g. 
    * _If an instance (host A) initiates traffic to host B and uses a protocol other than TCP, UDP, or ICMP, the instance's firewall only tracks the IP address & protocol number for the purpose of allowing response traffic from host B._
    * _If host B initiates traffic to the instance in a separate request within 600 seconds of the original request or response, the instance accepts it regardless of inbound security group rules, because it's regarded as response traffic._
* This can be controlled by modifying the security group's outbound rules to permit only certain types of outbound traffic. Alternatively, Network ACLs (NACLs) can be used for the subnet, network ACLs are stateless and therefore do not automatically allow response traffic.

## NACLs

* A Network ACLs (NACLs) is an optional layer of security for the VPC that acts as a firewall for controlling traffic in and out of one or more subnets.
* NACLs are not for granular control and are assigned at a Subnet level and is applicable to all the instances in that Subnet
* Network ACL has separate inbound and outbound rules, and each rule can either **allow or deny traffic**
    * Default ACL allows all inbound and outbound traffic.
    * Newly created ACL denies all inbound and outbound traffic
* A Subnet can be assigned only 1 NACLs and if not associated explicitly would be associated implicitly with the default NACL
* Network ACL is a numbered list of rules that are evaluated in order  
starting with the lowest numbered rule, to determine whether traffic is allowed in or out of any subnet associated with the network ACL  
_for e.g. if you have a Rule No. 100 with Allow All and 110 with Deny All, the Allow All would take precedence and all the traffic will be allowed_
* Network ACLs are **Stateless**; responses to allowed inbound traffic are subject to the rules for outbound traffic (and vice versa) _for e.g. if you enable Inbound SSH on port 22 from the specific IP address, you would need to add a Outbound rule for the response as well_![Screen Shot 2016-03-13 at 7.50.37 AM][7]


### Flow logs

* VPC Flow Logs is a feature that enables you to capture information about the IP traffic going to and from network interfaces in the VPC and can help in monitoring the traffic or troubleshooting any connectivity issues
* Flow log data is stored using Amazon CloudWatch Logs
* Flow log can be created for the entire VPC, subnets or each network interface. If enabled, for entire VPC or subnet all the network interfaces are monitored
* Flow logs do not capture real-time log streams for network interfaces.
* Flow logs can be created for network interfaces that are created by other AWS services; for example, Elastic Load Balancing, RDS, ElastiCache, Redshift, and WorkSpaces

## Subnets

* Subnet spans a single Availability Zone, distinct locations engineered to be isolated from failures in other AZs, and cannot span across AZs
* Subnet can be configured with an Internet gateway to enable communication over the Internet, or virtual private gateway (VPN) connection to enable communication with your corporate network
* Subnet can be Public or Private and it depends on whether it has Internet connectivity i.e. is able to route traffic to the Internet through the IGW
* Instances within the Public Subnet should be assigned a Public IP or Elastic IP address to be able to communicate with the Internet
* For Subnets not connected to the Internet, but has traffic routed through Virtual Private Gateway only is termed as VPN-only subnet
* Subnets can be configured to Enable assignment of the Public IP address to all the Instances launched within the Subnet by default, which can be overridden during the creation of the Instance
* **Subnet Sizing**
    * CIDR block assigned to the Subnet can be the same as the VPC CIDR, in this case you can launch only one subnet within your VPC
    * CIDR block assigned to the Subnet can be a subset of the VPC CIDR, which allows you to launch multiple subnets within the VPC
    * CIDR block assigned to the subnet should not be overlapping
    * CIDR block size allowed is between 
        * /28 netmask (minimum with 2^4 – 16 available IP address) and
        * /16 netmask (maximum with 2^16 – 65536 IP address)
    * AWS reserves 5 IPs address (first 4 and last 1 IP address) in each Subnet which are not available for use and cannot be assigned to an instance. _for e.g. for a Subnet with a CIDR block 10.0.0.0/24 the following five IPs are reserved_
        * _10.0.0.0: Network address_
        * _10.0.0.1: Reserved by AWS for the VPC router_
        * _10.0.0.2: Reserved by AWS for mapping to Amazon-provided DNS_
        * _10.0.0.3: Reserved by AWS for future use_
        * _10.0.0.255: Network broadcast address. AWS does not support broadcast in a VPC, therefore the address is reserved._
* **Subnet Routing**
    * Each Subnet is associated with a route table which controls the traffic.
* **Subnet Security**
    * Subnet security can be configured using Security groups and NACLs
    * Security groups works at instance level, NACLs work at the subnet level

## VPC Endpoints


# AWS VPC Endpoints - Certification

* VPC endpoint enables creation of a private connection between your VPC and another AWS service using its private IP address
* VPC Endpoint does not require a public IP address, access over the Internet, NAT device, a VPN connection or AWS Direct Connect
* Endpoints are virtual devices. They are horizontally scaled, redundant, and highly available VPC components that allow communication between instances in your VPC and AWS services without imposing availability risks or bandwidth constraints on your network traffic.
* Traffic between VPC and AWS service does not leave the Amazon network
* Endpoints currently do not support cross-region requests, ensure that the endpoint is created in the same region as your bucket
* AWS currently supports endpoints for S3 service only (**Update** – With latest enhancement DynamoDB is also supported)

![AWS VPC Endpoints][5]

## Configuration

* Endpoint requires the VPC and the service to be accessed via the endpoint
* Endpoint needs to be associated with the Route table and the route table cannot be modified to remove the route entry. It can only be deleted by removing the Endpoint association with the Route table
* A route is automatically added to the Route table with a destination that specifies the prefix list of service and the target with the endpoint id. for e.g. _A rule with destination **pl-68a54001 (com.amazonaws.us-west-2.s3)** and a target with this endpoints' ID (e.g. vpce-12345678) will be added to the route tables_
* Access to the resources in other services can be controlled by endpoint policies
* Security groups needs to be modified to allow Outbound traffic from the VPC to the service thats specified in the endpoint. Use the service prefix list ID for e.g. _com.amazonaws.us-east-1.s3_  as the destination in the outbound rule
* Multiple endpoint routes to different services can be specified in a route table, and multiple endpoint routes to the same service can be specified in different route tables, but you cannot have multiple endpoints to the same service in a single route table

## Limitations

* Endpoint cannot be created between a VPC and an AWS service in a different region.
* Endpoint cannot be tagged
* Endpoint cannot be transferred from one VPC to another, or from one service to another
* Endpoint connections cannot be extended out of a VPC i.e. resources across the VPN connection, VPC peering connection, AWS Direct Connect connection cannot use the endpoint




# AWS VPC Peering 


* A VPC peering connection is a networking connection between two VPCs that enables routing of traffic between them using private IP addresses.
* Instances in either VPC can communicate with each other as if they are within the same network
* VPC peering connection can be established between your own VPCs, or with a VPC in another AWS account within a single region.
* AWS uses the existing infrastructure of a VPC to create a VPC peering connection; it is neither a gateway nor a VPN connection, and does not rely on a separate piece of physical hardware. There is no single point of failure for communication or a bandwidth bottleneck.

## VPC Peering Rules & Limitations

**1\. **VPC peering connection cannot be created between VPCs that have matching or overlapping CIDR blocks.



**2\. **VPC peering connection cannot be created between VPCs in different regions. (**NOTE** – [VPC Peering is now supported inter-region.][6])  
**3\. **VPC peering connection are limited on the number active and pending VPC peering connections that you can have per VPC.

![VPC Peering Connections Limits][7]**4\. **VPC peering does not support transitive peering relationships   
In a VPC peering connection, your VPC does not have access to any other VPCs that the peer VPC may be peered with even if established entirely within your own AWS account![Screen Shot 2016-06-15 at 12.33.07 PM][8]**5\. **VPC peering does not support Edge to Edge Routing Through a Gateway or Private Connection

In a VPC peering connection, your VPC **does not** have access to any other connection that the peer VPC may have and vice versa. Connections that the peer VPC can include
* A VPN connection or an AWS Direct Connect connection to a corporate network
* An Internet connection through an Internet gateway
* An Internet connection in a private subnet through a NAT device
* A ClassicLink connection to an EC2-Classic instance
* A VPC endpoint to an AWS service; for example, an endpoint to S3.



**6\. **Only one VPC peering connection can be established between the same two VPCs at the same time  
**7\. **Maximum Transmission Unit (MTU) across a VPC peering connection is 1500 bytes.  
**8\. **A placement group can span peered VPCs; however, you do not get full-bisection bandwidth between instances in peered VPCs.  
**9\. **Unicast reverse path forwarding in VPC peering connections is not supported.  
**10\. **Instance's public DNS hostname does not resolve to its private IP address across peered VPCs.

## VPC Peering Architecture



* VPC Peering can be applied to create shared services or perform authentication with an on-premises instance
* This would help creating a single point of contact, as well limiting the VPN connections to a single account or VPC



