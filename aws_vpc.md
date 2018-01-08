# Networking Concepts
## What is a Network?
A network is two or more computers linked to share resources, exchange files, or allow electronic communications.

### Network Types:
* Local Area Network (LAN)
* Wide Area Network (WAN)
* Virtual Private Network (VPN)

### Local Area Network (LAN)
A LAN (local area network) is a group of computers and network devices connected together, usually within the same building. 
### Wide Area Network (WAN)
A WAN (wide area network), in comparison to a MAN, is not restricted to a geographical location, although it might be confined within the bounds of a state or country. A WAN connects several LANs, and may be limited to an enterprise (a corporation or an organization) or accessible to the public. 
### Virtual Private Network (VPN)
A virtual private network (VPN) is a technology that creates a safe and encrypted connection over a less secure network, such as the internet

## Physical vs. Logical Topology

![Network](network.png)
# VPC
## virtual private cloud
   * It is a logical data center, effectively a virtual network. 
   * Selection of your own IP address range, creation of subnets, and configuration of route tables and network gateways.
   * You can leverage multiple layers of security, including security groups and network access control lists to help control access to Amazon EC2 instances in each subnet. 
   * You can also create Hardware VPN connection between your corporate datacenter and your VPC to use AWS to extend your corporate datacenter.
   * Maximum addressable IP range is /16
   * Minimum addressable IP range is /28
   * It required CIDR block - Classless Inter Domain Routing - used to specify IP addresses

When creating a new VPC by default the following are created:
- Main route table
- Security Group
- Network ACL

the following are NOT created:
- Subnets
- Internet Gateway


## Default VPC

- Provided for each region for each account
- All Subnets have route to internet 
- if you delete VPC only way to restore is to contact AWS for support
- Tenancy - default (shared) or Dedicated

## VPC Peering

Connect one VPC with another via a direct network route using private IP addresses

- instance behave as if they are on the same private network
- can peer VPC's with other AWS accounts as well as with VPC's in the same account
- always Star configuration - no Transitive peering
## VPC Peering Rules & Limitations
-  VPC peering connection cannot be created between VPCs that have matching or overlapping
CIDR blocks.
-  VPC peering connection cannot be created between VPCs in different regions.
-  VPC peering connection are limited on the number active and pending VPC peering
connections that you can have per VPC.
-  VPC peering does not support transitive peering relationships
-  VPC peering does not support Edge to Edge Routing Through a Gateway or Private Connection


## Internet Gateway
- Attached to VPC
- Only one per VPC
- traffice to/from Internet

## Enable Internet Access through Internet GW
-  Attaching Internet gateway to the VPC
-  Subnet should have Route tables associated with the Route pointing to the Internet
gateway
-  Instances should have a Public IP or Elastic IP address assigned
-  Security groups and NACLs associated with the Instance should allow relevant traffic

## Virtual Private Gateway 

- traffic to/from VPN

## Route Tables

- Configured between subnets
- Destination => Target (CIDR block => instance Id)
- Default Route Table has not route out to Internet
## Security in Your VPC
- Security groups
- Network access control lists (ACLs)
- Key Pairs

## Network Access Control Lists
- Stateless - inbound rules not mirrored by outbound

## Security Group

- Stateful - inbound rules mirrored by outbound
- Defines Type, Protocol, Port Range and Source (one of CIDR, Security Group, IP address) combination allowed
- 
## Difference Between Region & Availability Zone
- Amazon EC2 is hosted in multiple locations world-wide.
- These locations are composed of regions and Availability Zones.
- Each region is a separate geographic area.
- Each region has multiple, isolated locations known as Availability Zones.
- Amazon EC2 provides you the ability to place resources, such as instances, and data in multiple locations. Resources aren't replicated across regions unless you do so specifically.

## Subnets

- Added to VPC
- Assign IP address ranges to (CIDR block)
- 1 Subnet = 1 Availability Zone
- Can be Private or Public Subnet
- Private IP address assigned by default
- Public IP address can be added to all instance if auto-assign Pubic IP address is turned on for Subnet
### So subnets come in two flavors, public and private. Here’s how they work:
- If a subnet’s traffic is routed to an Internet gateway, the subnet is known as a public subnet.
- If a subnet doesn’t have a route to the Internet gateway, the subnet is known as a private subnet.

## VPC Deletion:
Deletion of the VPC, possible only after terminating all instances within the VPC,deletes all the components with the VPC for e.g. subnets, security groups, network ACLs, route tables, Internet gateways, VPC peering connections, and DHCP options.

## Private IP Addresses
- Private IP addresses are not reachable over the Internet,and can be used for communication between the instances in your VPC
- All instances are assigned a private IP address, within the IP address range of the subnet, to the default network interface
- Primary IP address is associated with the network interface for its lifetime, even when the instance is stopped and restarted and is released only when the instance is terminated
- Additional Private IP addresses, known as secondary private IP address, can be assigned to the instances and these can be reassigned from one network interface to another

## Public IP address (Associated IP Address)
- Public IP addresses are reachable over the Internet, and can be used for communication between your instances and the Internet, or with other AWS services that have public endpoints
- Public IP address assignment to the Instance depends if the Public IP Addressing is enabled for the Subnet.
- Public IP address can also be assigned to the Instance by enabling the Public IP addressing during the creation of the instance, which overrides the subnet’s public IP addressing attribute
- Public IP address is assigned from AWS pool of IP addresses and it not associated with the AWS account and hence released when the instance is stopped and restarted

## Elastic IP address
- Elastic IP addresses are static, persistent public IP addresses which can be associated and
disassociated with the instance, as required
-  Elastic IP address is allocated at an VPC and owned by the account unless released
-  A Network Interface can be assigned either a Public IP or an Elastic IP. If you assign an
instance with Public IP an Elastic IP, the public IP is released
-  Elastic IP addresses can be moved from one instance to another and the instance can
be within the same VPC or different VPC within the same account
-  Elastic IP are charged for non usage i.e. if it is not associated or associated with a
stopped instance or an unattached Network Interface

## Elastic Network Interface (ENI)
-  Each Instance is attached with default elastic network interface (Primary Network
Interface eth0) and cannot be detached from the instance
-  ENI has the following attributes
- Primary private IP address
- One or more secondary private IP addresses
- One Elastic IP address per private IP address
- One or more security groups, A MAC address


## VPC Security
Security within a VPC is provided through
-  Security groups – Act as a firewall for associated Amazon EC2 instances, controlling
both inbound and outbound traffic at the instance level
-  Network access control lists (ACLs) – Act as a firewall for associated subnets,
controlling both inbound and outbound traffic at the subnet level
-  Flow logs – Capture information about the IP traffic going to and from network
interfaces in your VPC






## NAT

Network Address Translation  

### NAT Instances

- Available on Community Marketplace
- Deploy into Public subnet
- Requires Security Group
- Disable Source/Destination Check on Instance as acting as proxy
- Requires Route Table rule (Destination) 0.0.0.0/0 => (Target) NAT Instance Id (route out)
- Amount of traffice that NAT instance can supports, depends on instance size
- Problem: Single Point of Failure

### NAT Gateway

- Scales automatically and highly available
- No patching required
- Deploy into Public subnet
- Automatically assigned public IP address
- No Security Group
- Requires Route Table rule (Destination) 0.0.0.0/0 => (Target) NAT Gateway Id


