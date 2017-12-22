# CLoudwatch
Aws CLoudwatch is a monitoring service for aws cloud resources and the appications you run on AWS.

## Monitoring 

### Basics monitoring
    * Is Free
    * Polls every 5 mins
    * 10 metrics
    * 5Gb data ingestio
    * 5Gb data storage
### Detailed monitoring 
	* Is chargabel
	* charged per instance per month
	* polls every minute
## Metrics
Aws CloudWatch allows you to record metrics for EBs,EC2,ELB,and S3


## Events

Can create events based on cloudwatc monitoring, for example trigger Lambda function.


aws-->lambda-->archive

## Logs
Install agents on Ec2 instances to send monitoring data about the instance to Cloudwatch.

ec2 instances ---->cloud watch       exceptin in appilt
## Alarms
Set alarms to warn based on resources usage, for example CPU utilization is to high.


