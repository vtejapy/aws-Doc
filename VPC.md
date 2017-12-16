# VPC

Virtual Private Cloud  

It is a logical data center, effectively a virtual network. Selection of your own IP address range, creation of subnets, and configuration of route tables and network gateways. You can leverage multiple layers of security, including security groups and network access control lists to help control access to Amazon EC2 instances in each subnet. You can also create Hardware VPN connection between your corporate datacenter and your VPC to use AWS to extend your corporate datacenter.

- VPCs do not span Regions but can span Availability Zones
- Maximum addressable IP range is /16
- CIDR block - Classless Inter Domain Routing - used to specify IP addresses

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

## Internet Gateway

- Attached to VPC
- Only one per VPC
- traffice to/from Internet

## Virtual Private Gateway 

- traffic to/from VPN

## Route Tables

- Configured between subnets
- Destination => Target (CIDR block => instance Id)
- Default Route Table has not route out to Internet

## Network Access Control Lists

- Stateless - inbound rules not mirrored by outbound

## Security Group

- Stateful - inbound rules mirrored by outbound
- Defines Type, Protocol, Port Range and Source (one of CIDR, Security Group, IP address) combination allowed
- 

## Subnets

- Added to VPC
- Assign IP address ranges to (CIDR block)
- 1 Subnet = 1 Availability Zone
- Can be Private or Public Subnet
- Private IP address assigned by default
- Public IP address can be added to all instance if auto-assign Pubic IP address is turned on for Subnet

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


