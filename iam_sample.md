## AWS Identity and Access Management 
   - Access control for AWS services and resources that is flexible, powerful, familiar, and secure.
   - Flexible
     - Individual use
     - Organizations
     - Enterprise
   - Powerful
     - Integrated
     - Fine-grained
     - Delegation
     - Scale
   - Familiar
     - Administration
     - Enterprise federation
     - Web identity federation
   - Secure
     - Powerful controls
     - Audit
     - Compliance

## Flexible
   – $: No upfront investment, free tier, low ongoing cost
   – Scale: Flexible capacity, global reach
   – Agility: Speed and agility, apps not ops
   – Services: Amazon EC2, Amazon S3, Amazon DynamoDB,Amazon Redshift, Amazon RDS, Amazon EMR, Amazon CloudFront,.. etc.
## How many initially tried AWS because of
   – Security
   – Identity

### Flexible  Individual Use

### Flexible  Organizations


## IAM
   - Users, groups, permissions
     - Individual security credentials
     - Secure by default
     - Grant least privilege
   - Easy to use
     - Graphical user interface
     - Ability to script/automate (CLI & API)

Flexible  Enterprise

## Control
   - AWS multi-factor authentication
     - Hardware tokens
     - Smartphone app tokens
   - Credential management policies
     - Control billing, support, and AWS Marketplace purchases

### Powerful Integrated
   - Cloud Services
   - Cloud Resources

### Powerful Fine-Grained

Amazon EC2 Resource-Level Permissions

Example use cases:

• Ben can terminate instance i-abc12345 but not
instance i-def67890
• Jeff can launch instances only in the subnet
subnet-bdf2468
• Ken can use only the AMI ami-cba54321 to run
instances
• A user can take any action on resources if they
have the tag “sandbox=${aws:username}”
• Derek must authenticate using MFA before he can
terminate instances with the tag “stack=prod”


Powerful Delegation

IAM Role
• Entity that defines a set of permissions
• Not associated with a specific user or
group
• Roles must be “assumed” by trusted
entities


IAM Roles for Amazon EC2
• Allow Amazon EC2-based apps to act on behalf of
another entity
• Create a role, apply a policy, launch instance with role
• Credentials are automatically:
– Made available to Amazon EC2 instances
– Rotated multiple times a day

Benefits of Using Roles with Amazon EC2
• Eliminates use of long-term credentials
• Automatic credential rotation
• Less coding – AWS SDK does all the work
• Easier and more Secure!

Powerful Scale


Trillions
Resources
Million+
Requests/Second

Familiar  Administration

IAM Policy Simulator
• Test the effect of access control policies before
pushing to production
• Verify and troubleshoot permissions


Familiar  Enterprise Federation

Federation
• AWS websites and/or APIs as relying party
• Pre-packaged samples: Windows Active Directory, Shibboleth


SSO Federation Using SAML
• STS now supports SAML 2.0
• Benefits:
– Open standards
– Quicker and easier to implement federation
– Leverage existing identity management software to manage access to AWS resources
– No coding required
• AWS Management Console SSO
– IdP-initiated web SSO via SAML 2.0 using the HTTP-POST binding (web SSO profile)
– New sign-in URL that greatly simplifies SSO
 https://signin.aws.amazon.com/saml<SAML AuthN response>
• API federation using new assumeRoleWithSAML operation



Familiar  Web Identity Federation

Web Identity Federation
• App sign-in using 3rd party identity providers
– Login with Amazon
– Facebook
– Google
• Apps can access data from
– Amazon S3, Amazon DynamoDB, Amazon Simple Notification
Service (now with mobile push!)
• No server-side code required


### Secure  Powerful Controls
Delegate Access Across Accounts
- Access resources across AWS accounts
- Why do you need it?
– Management visibility across all your AWS accounts
– Developer access to resources across AWS accounts
– Use third-party solutions, with no sharing of credentials



