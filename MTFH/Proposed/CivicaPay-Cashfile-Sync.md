# CivicaPay Cashfile Sync

### **Date:** 30 May 2022

### **Status:** PROPOSED 

## **Context**
The CivicaPay cash files are required to be imported into the Interim Finance System (IFS) on a daily basis. Currently the CivicaPay system generates a cash file (.dat format) at 7PM daily. On the following morning this is then copied to a Google Drive folder where it is processed by an AWS Step Function.

The drawback to this method is that on the Finance side this process is manually driven and is therefore prone to error. 

## **Decision**
This is a proposal for the infrastructure required to automate this process.

![Proposed infrastructure](https://drive.google.com/uc?export=view&id=1Rh36VrucgNT04F0PRj3cGPaVV4-Ddy-_)


The following AWS resources need to be provisioned:

-   S3 Bucket
    -   contains a folder named cashfiles/
    -   Lifecycle policy to expire objects older than 30 days
-   SFTP server endpoint for the S3
    -   User with SSH access (note that the username and public key are stored in Parameter store)
-   IAM Role and attached Permission Policy
    -   Write to the S3 bucket - for the SFTP client
    -   Read (GetObject) - for the Cashfile Lambda function
-   Lambda Trigger event to respond to PUT events on the S3 bucket

**Process:**
1.  The CivicaPay System SFTP client performs a PUT transfer of the .dat cash file to the SFTP endpoint using a user SSH private key. This is saved to a folder (/cashfiles) of the S3 bucket. This creates an ObjectCreated event on the S3 bucket.
2.  Add a new Trigger to the Lamda function which is configured to respond to the ObjectCreated event on the S3 for the cashfiles/ folder for objects with an .dat extension.
3.  The Lambda function then copies the file to the Finance Google Drive folder and processes and imports the data into the IFS system.

## **Further details** 
A working version of the process is currently operational on the Housing Finance development environment. 

## **Consequences**
The current process requires daily manual intervention by a Housing Finance Officer. This process removes that dependency on manual intervention and the risks thereof.