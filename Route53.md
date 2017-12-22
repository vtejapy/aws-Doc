## Route 53

### Amazon Route 53 provides three main functions:
   * Domain registration
     * allows you to register domain names
   * Domain Name System (DNS) service
      * translates friendly domains names like www.example.com into IP addresses like 192.0.2.1
      * responds to DNS queries using a global network of authoritative DNS servers, which reduces latency
      * can route Internet traffic to CloudFront, Elastic Beanstalk, ELB, or S3. There’s no charge for DNS queries to these resources
   * Health checking
       * can monitor the health of resources such as web and email servers.
       * sends automated requests over the Internet to the application to verify that it’s reachable, available, and functional
       * CloudWatch alarms can be configured for the health checks to send notification when a resource becomes unavailable.
       * can be configured to route Internet traffic away from resources that are unavailable


### Supported DNS Resource Record Types

   * A (Address) Format
       * is an IPv4 address in dotted decimal notation for e.g. 192.0.2.1
   * AAAA Format
       * is an IPv6 address in colon-separated hexadecimal format
   * CNAME Format
       * is the same format as a domain name
       * DNS protocol does not allow creation of a CNAME record for the top node of a DNS namespace, also known as the zone apex for e.g. the DNS name example.com registration, the zone apex is example.com, a CNAME record for example.com cannot be created, but CNAME records can be created for www.example.com, newproduct.example.com etc.
       * If a CNAME record is created for a subdomain, any other resource record sets for that subdomain cannot be created for e.g. if a CNAME created for www.example.com, not other resource record sets for which the value of the Name field is www.example.com can be created
   * MX (Mail Xchange) Format
       * contains a decimal number that represents the priority of the MX record, and the domain name of an email server
   * NS (Name Server) Format
        * An NS record identifies the name servers for the hosted zone. The value for an NS record is the domain name of a name server.
   * PTR Format
        * A PTR record Value element is the same format as a domain name.
   * SOA (Start of Authority) Format
       * SOA record provides information about a domain and the corresponding Amazon Route 53 hosted zone
   * SPF (Sender Policy Framework) Format
       * SPF records were formerly used to verify the identity of the sender of email messages, however is not recommended
       * Instead of an SPF record, a TXT record that contains the applicable value is recommended
   * SRV Format
       * An SRV record Value element consists of four space-separated values.The first three values are decimal numbers representing priority, weight, and port. The fourth value is a domain name for e.g. 10 5 80 hostname.example.com
   * TXT (Text) Format
       * A TXT record contains a space-separated list of double-quoted strings. A single string include a maximum
of 255 characters. In addition to the characters that are permitted unescaped in domain names, space
is allowed in TXT strings

### Alias resource record sets

   * Route 53 supports alias resource record sets, which enables routing of queries to a CloudFront distribution, Elastic Beanstalk, ELB, an S3 bucket configured as a static website, or another Route 53 resource record set
   * Alias records are not standard for DNS RFC and are an Route 53 extension to DNS functionality
   * Alias records help map the apex zone (root domain without the www) records to the load balancer DNS name as the DNS specification requires “zone apex” to point to an ‘A’ record (ip address) and not to an CNAME
   * Route 53 automatically recognizes changes in the resource record sets that the alias resource record set refers to for e.g. for a site pointing to an load balancer, if the ip of the load balancer changes, Route 53 will reflect those changes automatically in the DNS answers without any changes to the hosted zone that contains resource record sets
   * If an alias resource record set points to a CloudFront distribution, a load balancer, or an S3 bucket, the time to live (TTL) can’t be set; Route 53 uses the CloudFront, load balancer, or Amazon S3 TTLs.
