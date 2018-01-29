# Elastic Beanstack

A Service for deploying and scalling web application and services.
Upload your code and Elastic Beanstack automatically handles the deployment, from capacity provisioning, load balancing, auto-scalling to application health monitoring 

##overview:

  * Integrate with vpc
  * Integrate with Iam
  * can provision rds instances
  * full contro lof resources
  * code is stored in s3
  * multiple env are suported to enable versioningchanges from git repo are repicated 
  * linux and windows 2008  r2 ami support
  * deploy code using a war file or git repo
  * elastic beanstalk is fault tolreant with i n a single region(not ft between regions)

  * by default applications are publicly accessable


## elastic Beanstack management
   * cloudwatch monitoring
   * adjust applications server settings
   * run other appliactions components
   * access log files without logging into application server

## clouformation vs Elastic beanstalk 

   * cloudformation supports elastic beanstalk

   * elastic beanstalk doesn't provision cloudformation templates

   * elastic beanstalk is ideal for deverlpers with limited cloud experience that needs to deploy env fast

   * elastic beanstalk is ideal if you hav a standard php , java,ruby, node.js,.net,go, or docker applications that can run on an app server with a databases
