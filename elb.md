# Elastic Load Balancer

Elastic Load Balancing automatically distributes incoming application traffic across multiple targets, such as Amazon EC2 instances, containers, and IP addresses. 

## Elastic Load Balancing supports three types of load balancers:
   * Application Load Balancers
   * Network Load Balancers
   * Classic Load Balancers 

## understanding of OSI layer

### Layer 4(Network)
    * support TCP and SSL
    * Incoming Client Connection bound to server connection.
    * proxy protocol prepared source and destination IP and PORT to requests
### Layer 7 (Application)
    * Support HTTP and HTTPS
    * COnnection terminated at the load balancer and pooled to the server

## The Elastic Load Balancing family

### Application Load Balancer
    * HTTP and HTTPS (VPC)
### Network Load Balancer
    * TCP Workload (VPC)
### Classic Load Balancer
    * For HTTP,HTTPS and TCP


