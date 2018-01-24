# RDS


##  Managed service RDS vs. a self-hosted database on virtual servers
 
 
|     | Amazon RDS          | Self-hosted on virtual servers  |
| ------------- |:-------------:| -----:|
| Cost for AWS    |services Higher because RDS costs more than virtual servers (EC2) | Lower because virtual servers (EC2) are cheaper than RDS |
| Total cost of ownership    | Lower because operating costs are split among many customers     |  Much higher because you need your own manpower to manage your database|
| Quality | AWS professionals are responsible for the managed service.     |    You’ll need to build a team of professionals and implement quality control yourself. |
| Flexibility | High, because you can choose a rela-tional database system and most of the configuration parameters |  Higher, because you can control every part of the relational database system you installed on virtual servers |




## Attributes needed to connect to an RDS database


| Attribute        | Description    |     
| ------------- |:-------------:| 
| AllocatedStorage     | Storage size of your database in GB, also known as instance type  | 
| DBInstanceClass     | size, of the underlying virtual server      |   
| Engine | Database engine (MySQL, Oracle Database, Microsoft SQL Server,or PostgreSQL) you want to use      |  
DBInstanceIdentifier | Identifier for the database instance |
DBName | Identifier for the database | 
MasterUsername | Name for the master user |
MasterUserPassword | Password for the master user |




## Attributes needed to connect to an RDS database


| Attribute        | Description    |     
| ------------- |:-------------:| 
| Endpoint     | Host name and port of the database endpoint needed to connect your applications to the database. This interface receives SQL commands. | 
| DBName     | Name of the default database that’s automatically created at launch.     |   
| MasterUsername | Name of the master user for the database. The password isn’t shown again; you have to remember it or look it up in the CloudFormation template. The master user can create additional database users. The handling depends on the underlying database. |  
Engine  | Describes the relational database system offered by this RDS instance.|

 
 


 
