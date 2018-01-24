# rds 

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

 
 
