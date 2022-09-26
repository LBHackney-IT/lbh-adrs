# Housing-Finance Database Migration 

  

### **Date:** 23 September 2022 

### **Status:**  PROPOSED

## **Context**
The data migration project to transfer data from the IFS database to the microservice databases for the Housing Finance APIs was not completed by the previous agency. The IFS database is an AWS RDS Microsoft SQL Server instance which has high operational costs in comparison to other RDS database services available. The context of this proposal is to address both of these issues.
 

## **Vision**
![Proposed Database Infrastructure](https://drive.google.com/uc?export=view&id=1VqsLruA7lLN4AphIU9xXuyiAvBA83ZZp)
The vision is to convert the database schema and the database objects using a heterogeneous database migration plan, with the intention to deploy the following database architecture and services:
|**Database Identifier**| **Engine**|**Service**|
|--|--|--|
[Primary Identifier]: housing-finance-postgres-master-db-[env] | PostgresSQL |HFS Ingest |
[Replica 1 Identifier]: housing-finance-postgres-replica-db-01-[env] | PostgresSQL |RentSense |
[Replica 2 Identifier]: housing-finance-postgres-replica-db-02-[env] | PostgresSQL |Accounts API |
[Replica 3 Identifier]: housing-finance-postgres-replica-db-03-[env] | PostgresSQL |Charges API |
[Replica 4 Identifier]: housing-finance-postgres-replica-db-04-[env] | PostgresSQL |Transactions API |

A number of steps have been identified as necessary for

 1. Create the Primary PostgresSQL and Replica instances using Terraform
 2. Convert the Schema of the MSSQL Primary RDS to PostgresSQL Schema conversion is done using the AWS Schema Conversion Tool (SCT)
 3. Migrate the data from MSSQL to PostgresSQL using AWS Data Migration Services (DMS) by operating the Full Data Load and Continuing Data Changes (CDC) migration type
 4. Repoint the Accounts, Charges and Transactions APIs to the read-replica instances

### Create the Primary and Replica instances:

These five database instances have been provisioned and maintained in the `mtfh-finance-infrastructure` Github repository. The pull request for the proof-of-concept infrastructure for this proposal can be found here:
[https://github.com/LBHackney-IT/mtfh-finance-infrastructure/pull/14](https://github.com/LBHackney-IT/mtfh-finance-infrastructure/pull/14)

**Convert Schema: MSSQL to PostgresSQL**
![AWS SCT](https://drive.google.com/uc?export=view&id=1bVvBocBdip2ZtILZgKJodZm4sbS5JiBA)
To complete the Schema migration, we have used the AWS Schema Conversion Tool (SCT). The SCT is a desktop application which helps heterogeneous database migrations by examining the source database schema and automatically converting as much of the database objects as possible, including tables, views, stored procedures and functions from, in this case, the source MS SQL Server to PostgresSQL. Any objects that the SCT can’t convert are marked with detailed information on how the object can be converted manually. 

The User Guide for the SCT in PDF format can be found [here](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/Schema-Conversion-Tool.pdf).
#### Results of the SCT Migration Tool
| **Database Object** |**Number (Source)**  | **Automatically Converted (Target)** |
|--|--|--|
|Tables  | 93 | 93 |
|Views| 5| 3 |
|Stored Procedures | 39 | 2|
|Scalar Functions| 6| 3|

#### The SCT Database Assessment Report Findings

The SCT creates a complete analysis report of the schema conversion it undertakes following a successful analysis of the source to target databases. The findings are produced as a separate report in PDF, which is found here. Excerpts from the report are shown here:

> 157 (100%) database storage object(s) and 16 (32%) database code
> object(s) that can be converted to Amazon RDS for PostgreSQL
> automatically or with minimal changes. 
> We found 2 encrypted object(s).
> 34 (68%) database code object(s) require 510 complex user action(s) to
> complete the conversion

#### Conversion Analysis

![Conversion Complexity Charts](https://drive.google.com/uc?export=view&id=1CTx4LjYuWATNDFHvWHTmW86xYOzjAYeU)

The SCT Assessment Report also provides detailed analysis of the SQL code and highlights not only where the SQL code could not be converted but also pinpoints why it was unable to complete the conversion of the code on an object level. So for example, the following visual analysis is provided for each stored procedure which cannot be converted, to aid the development team to manually complete the conversion. This will take developer effort in order to manually convert these stored procedures and views to PostgresSQL.

A screenshot of the code analysis view is shown below.
![enter image description here](https://drive.google.com/uc?export=view&id=18ZO27IRQo2PUo5gEXgm0RcdGHqV0Dr91)

### Migrating the Data
Following the completion of the Schema conversion, the next step would be to migrate the data from the Primary SQL Server to the Primary (master) Postgres RDS instance. This has been done in the POC using AWS Data Migration Services (DMS).

The following data tables have been identified as mandatory for servicing the APIs:

**Accounts API tables**
 - UHTenancyAgreement
 - MATenancyAgreement
 - UHMember
 - MAMember
 - UHHousehold
 - TenancyAgreementAUX
 - TenancyAgreementHistory

**Charges API tables**
 - Charges
 - ChargesAUX
 - ChargeBatchYear
 - ChargeHistory
 - UHDebitItem (legacy)

**Transaction API tables**
 - UHMiniTransaction (legacy)
 - SSMiniTransaction (new)

### Applying Data Migration Services
The following is a step by step to complete the data migration using DMS
1.  Configure the source MS SQL Server db for migration
2.  Configure the target PostgresSQL db for migration
3.  In AWS Management console DMS, create the following resources:
			- Create an DMS Replication instance
			- Create the DMS Source and Target endpoints
			- Create and run the DMS Migration Task

#### Configure the source MS SQL Server db for migration
    USE [sow2b]
    -- Enable CDC replication
    EXEC msdb.dbo.rds_cdc_enable_db 'sow2b'
    
    -- remove the failing cursor error
    SET NOCOUNT ON
    
    -- prime for CDC
    ALTER DATABASE [sow2b] SET RECOVERY FULL WITH NO_WAIT
    
    -- the following command might be needed for all tables, just ran it for this one
    EXEC sys.sp_cdc_enable_table @source_schema=N'dbo', @source_name=N'ActionDiaryAux', @role_name=NULL;
    
    -- increasing the polling time from 5s to 1200s
    EXEC sp_cdc_change_job @job_type='capture', @maxtrans=500, @maxscans=10, @continuous=1, @pollinginterval=1200

#### Configure the target PostgresSQL db for migration
In the target Postgres database, run the following commands for each table that is going to be migrated and which has a constraint:

    ALTER  TABLE  [schema_name].[table_name]
    DROP  CONSTRAINT  IF  EXISTS  [constraint_name]

### Create an DMS Replication instance
Login to the AWS Management Console and in AWS DMS, create a Replication instance with the following parameters:
| **Form Parameter** | **Value** |
|--|--|
| **Task identifier** | replication-instance-mssql-postgres |
| **Migration type** | Select “Migrate existing data and replicate ongoing changes” |
| **Instance class** | Select “dms.c5.xlarge” |
| **Engine Version** | 3.4.7 |
| **Multi AZ** | Production workload (Multi-AZ) |
| **Publicly Accessible** | No |
| **Availability Zone** | eu-west-2a |

### Create the DMS Source and Target endpoints
Create the following endpoints:
**Source Endpoint (MS SQL Server)**
| Form Parameter | Value |
|--|--|
| **Endpoint Identifier** | endpoint-ifs-mssql-source |
| **Source Engine** | Microsoft SQL Server |
| **Access to endpoint database** | Provide access information manually |
| **Server name** | [database server] |
| **Port** | 1433 |
| **User name** | housingadmin |
| **Password** | **** |
| **SSL mode** | none |
| **Database name** | sow2b |

**Target Endpoint (PostgresSQL)**
| Form Parameter | Value |
|--|--|
| **Endpoint Identifier** | endpoint-hfs-postgres-target |
| **Source Engine** | PostgresSQL |
| **Access to endpoint database** | Provide access information manually |
| **Server name** | [database server] |
| **Port** | 1433 |
| **User name** | financeadmin |
| **Password** | **** |
| **SSL mode** | none |
| **Database name** | housingfinance |

### Create and run the DMS Migration Task
Create a Data Migration Task with the following parameters:
| **Form Parameter** | **Value** |
|--|--|
| **Replication Instance** | replication-instance-mssql-postgres |
| **Source Endpoint** | endpoint-ifs-mssql-source |
| **Target Endpoint** | endpoint-hfs-postgres-target |
| **Migration Type** | Select option “Migrate existing data and replicate ongoing changes" |

#### Table Mappings

| **Selection Rule 1** |  |
|--|--|
| **Schema** | Select option “Enter a schema” |
| **Schema name** | dbo |
| **Table name** | % |
| **Action** | Select option "Include" |
.
| **Transformation Rule 1**|  |
|--|--|
| **Rule Target** | Select option "Schema" |
| **Source name** | Select “Enter a schema” |
| **Source value** | dbo |
| **Action** | Select option "Rename to" |
| **Value** | housingfinance_dbo |
.
| **Transformation Rule 2**|  |
|--|--|
| **Rule Target** | Select option "Schema" |
| **Source name** | Select option "Enter a Table" |
| **Source value** | dbo |
| **Action** | Make lowercase |
.
| **Transformation Rule 3**|  |
|--|--|
| **Rule Target** | Select option "Table" |
| **Source name** |Select option "Enter a schema"  |
| **Source value** | dbo |
| **Table Name** | % |
| **Value** | Make lowercase |
.
| **Transformation Rule 4**|  |
|--|--|
| **Rule Target** | Select option "Column" |
| **Source name** | Select option "Enter a schema" |
| **Source value** | dbo |
| **Table Name** | % |
| **Column Name** | % |
| **Action** | Make lowercase |
.


## **Decision**
The following has been completed and is in process:

 - Creation of the PostgresSQL primary-replica infrastructure completed on Development and Staging environments.
 - We have migrated the data from the IFS database and it is now replicating with the PostgresSQL master database for ongoing changes.
 - We will be conducting consultations with other MTFH teams to discuss and agree on the exposed data-models of the Finance APIs.
 - We are in the process of repointing the APIs to the new dedicated Postgres replica instances.
 - We will be proposing additional features such as Caching data retrieval and implementation of CQRS at the API level
  

## **Further details**

 - [Migrating a commercial database to open source with AWS SCT and AWS DMS](https://aws.amazon.com/blogs/database/migrating-a-commercial-database-to-open-source-with-aws-sct-and-aws-dms/)
 - [AWS Schema Conversion Tool User Guide](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/Schema-Conversion-Tool.pdf)
 - [AWS Data Migration Service User Guide](https://docs.aws.amazon.com/dms/latest/userguide/Welcome.html)
 - [Migrate SQL Server to Amazon Aurora PostgreSQL using best practices and lessons learned from the field](https://aws.amazon.com/blogs/database/migrate-sql-server-to-amazon-aurora-postgresql-using-best-practices-and-lessons-learned-from-the-field/)
 - [Step-by-Step Guide for Microsoft SQL Server Migration to Amazon Aurora (MySQL)](https://github.com/aws-samples/aws-dms-getting-started-blog/blob/main/SQLServerMigration.md) 
 - [Step-by-Step Guide for Oracle Migration to Amazon Aurora (PostgreSQL)](https://github.com/aws-samples/aws-dms-getting-started-blog/blob/main/OracleMigration.md)
  

## **Consequences**
 

This change will lower the monthly operational cost of using SQL Server of the IFS database and the the DynamoDb databases. It will complete the database migration of IFS to the individual microservice databases for the Housing Finance APIs (Accounts, Charges and Transactions) which will enable other applications to consume this data.