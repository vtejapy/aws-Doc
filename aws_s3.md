## what is s3 ?
   * Amazon Simple Storage Service (Amazon S3) is storage for the Internet. 
   * You can use Amazon S3 to store and retrieve any amount of data at any time, from anywhere on the web.
   * It is designed for large-capacity, low-cost storage provision across multiple geographical regions. 
   * Amazon S3 provides developers and IT teams with Secure, Durable and Highly Scalable object storage.
   * In S3, there’s no initial charges, zero setup cost. You only pay for what you utilize.

## S3 is Secure because AWS provides:
   * Encryption to the data that you store. It can happen in two ways:
     - Client Side Encryption
     - Server Side Encryption
   * Multiple copies are maintained to enable regeneration of data in case of data corruption
   * Versioning, wherein each edit is archived for a potential retrieval.
## S3 is Durable because:

   * It regularly verifies the integrity of data stored using checksums e.g. if S3 detects there is any corruption in data, it is immediately repaired with the help of replicated data.
   * Even while storing or retrieving data, it checks incoming network traffic for any corrupted data packets.
S3 is Highly Scalable, since it automatically scales your storage according to your requirement and you only pay for the storage you use.


## Who needs Amazon S3?

### Running out of bandwidths

If you are on shared hosting account, any Stumble Upon or Digg effect can easily eat up the entire bandwidth limit for the month. Most of the time, the web host will suspend the account until you have settle the payment for the extra bandwidths consumed. Amazon S3 provides unlimited bandwidth and you’ll be served with any amount of bandwidth your site needs.

### Better scalability

Amazon S3 using cloud hosting and image serving is relatively fast. Separating them away from normal HTTP request will definitely ease the server load and thus, guarantees better stability.

### Paying for more that you actually used

Whether you are on shared hosting, VPS or dedicated server, you pay a lump sum each month (or year) and the amount includes hard disk storage and bandwidth you might not fully make use of. Why pay for more when you can pay only for what you are used.

### Store files online

nstead of backing up your files in CD/DVDs to save more hard disk space, here’s another option. Store them online, and you have the option to keep them private or make them public accessible.


### Easier files retrieval and sharing

If you store your file online, you can access them anywhere as long as there’s Internet connection. Amazon S3 also allows me to communicate files better with friends, clients, and blog readers.

## Bucket Naming
   * bucket names must be 3 characters long and more than 63 characters
   * bucket names must be serious of one or more labels
   * aws recommends separationg labels witha single period(.)
   * bucket names can contain lower case letters,numbers and hyphens
   * Each label must start and end with a lowercase letter or a number.

eg:
```
mybucket.1
myawsbucket
my.aws.bucket
```

## Bucket Restrictions
   * you can create maximum 100 buckets in each of your aws account
   * you can't transfer the ownership of a bucket.
   * you can store unlimited number of objects in a bucket
   * you cant create a bucket within another bucket.








