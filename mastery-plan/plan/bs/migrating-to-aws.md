# Migrating to AWS

These are all the services which can be used to migrate your on premise or other CLoud platform resources to AWS.

* AWS Migration Hub
* AWS Application Discovery Service 
* AWS Database Migration Service
* AWS Server Migration Service
* AWS Snowball
* AWS Snowball Edge
* AWS Snowmobile
* AWS DataSync
* AWS Transfer for SFTP

### AWS Migration Hub:

AWS Migration Hub provides a single location to track the progress of application migrations across multiple AWS and partner solutions. Using Migration Hub allows you to choose the AWS and partner migration tools that best fit your needs, while providing visibility into the status of migrations across your portfolio of applications.

AWS Migration Hub provides a single place to monitor migrations in any AWS region where your migration tools are available. There is **no additional cost** for using Migration Hub. You **only pay for the cost of the individual migration tools you use**, and any resources being consumed on AWS.

* This is how the main dashboard looks like:

![](https://thor1345r75.s3.ap-south-1.amazonaws.com/migration+services/migration2.png)

* You have two options to migrate:
  * Perform Discovery and Then Migrate
  * Migrate Without Performing Discovery
* Once you click on discovery first then this window appears. This is the flow in which the migration will be done. Click _**Use discovery tools**_ ![](https://thor1345r75.s3.ap-south-1.amazonaws.com/migration+services/migration3.png)
* Which should take you to these discovery tools. Discovery connector is for VMware machines. 
* AWS Discovery Agent is a tool provided by AWS which you can install on either Linux/Windows machines. 

![](https://thor1345r75.s3.ap-south-1.amazonaws.com/migration+services/migration4.png)

* If you choose the import option instead: ![](https://thor1345r75.s3.ap-south-1.amazonaws.com/migration+services/migration6.png)
  * You have to download a sample template provided by AWS. Which looks like this.Fill it up and upload to S3 .Provide the S3 object url and click Import.![](https://thor1345r75.s3.ap-south-1.amazonaws.com/migration+services/migration5.png) 
  * 
* Next these are the migration tools you can use to migrate the discovered services. ![](https://thor1345r75.s3.ap-south-1.amazonaws.com/migration+services/migration7.png)

**Helpful Link**

> [AWS Migration Hub Walkthroughs - AWS Migration Hub](https://docs.aws.amazon.com/migrationhub/latest/ug/walkthroughs.html)

### AWS Application Discovery Service

AWS Application Discovery Service helps you plan your migration to the AWS cloud by collecting usage and configuration data about your on-premises servers. Application Discovery Service is **integrated with AWS Migration Hub**, which simplifies your migration tracking.

After performing discovery, you can view the discovered servers, group them into applications, and then track the migration status of each application from the Migration Hub console.

Two ways of performing discovery and collecting data about your on-premises servers:

* Agentless discovery
* Agent-based discovery

### AWS Database Migration Service

AWS Database Migration Service helps you migrate databases to AWS quickly and securely. The source **database remains fully operational during the migration**, minimizing downtime to applications that rely on the database.

AWS Database Migration Service supports **homogeneous migrations** such as Oracle to Oracle, as well as **heterogeneous migrations** between different database platforms, such as Oracle or Microsoft SQL Server to Amazon Aurora. With AWS Database Migration Service, you can continuously replicate your data with high availability and consolidate databases into a petabyte-scale data warehouse by streaming data to Amazon Redshift and Amazon S3.

* AWS DMS migrates data, tables, and primary keys to the target database. All other database elements are not migrated.
* The AWS Schema Conversion Tool \(SCT\) makes heterogeneous database migrations easy by automatically converting the source database schema and a majority of the custom code, including views, stored procedures, and functions, to a format compatible with the target database. 

![](https://thor1345r75.s3.ap-south-1.amazonaws.com/migration+services/migration9.png)

### AWS Server Migration Service

AWS Server Migration Service \(SMS\) is an agentless service which makes it easier and faster for you to migrate thousands of on-premises workloads to AWS. AWS SMS allows you to automate, schedule, and track incremental replications of live server volumes, making it easier for you to coordinate large-scale server migrations.

* Currently, you can migrate virtual machines from VMware vSphere and Windows Hyper-V to AWS using AWS Server Migration Service.
* AWS Server Migration Service supports migrating Windows Server 2003, 2008, 2012, and 2016, and Windows 7, 8, and 10; Red Hat Enterprise Linux \(RHEL\), SUSE/SLES, CentOS, Ubuntu, Oracle Linux, Fedora, and Debian Linux operating systems.

![](https://thor1345r75.s3.ap-south-1.amazonaws.com/migration+services/migration8.png)

### AWS Snowball

AWS Snowball is a petabyte-scale data transport solution that uses secure appliances to transfer large amounts of data into and out of AWS.

With Snowball, you **don’t need to write any code or purchase any hardware to transfer your data**. Simply create a job in the AWS Management Console and a Snowball appliance will be automatically shipped to you.

Once it arrives, attach the appliance to your local network, download and run the Snowball client to establish a connection, and then use the client to select the file directories that you want to transfer to the appliance.

![](https://thor1345r75.s3.ap-south-1.amazonaws.com/migration+services/migration10.png)

Here you can either choose that the data you send in the device is sent to AWS S3 or data on Amazon S3 is put on a device and that is shipped to you or use the device for local compute and storage ![](https://thor1345r75.s3.ap-south-1.amazonaws.com/migration+services/migration11.png)

On the next slides enter your data, give job details, set teh security /notifications and you are all set

### AWS Snowball Edge

AWS Snowball Edge is a data migration and edge computing device that comes in **two** options. Snowball Edge Storage Optimized provides 100 TB of capacity and 24 vCPUs and is well suited for local storage and large scale data transfer. Snowball Edge Compute Optimized provides 52 vCPUs and an optional GPU

### AWS Snowmobile:

AWS Snowmobile is an **exabyte-scale data transfer service** used to move extremely large amounts of data to AWS. You can transfer up to **100 PB per Snowmobile**, a 45-foot long ruggedized shipping container, pulled by a semi-trailer truck.

### AWS DataSync

AWS DataSync is a data transfer service that makes it easy for you to **automate moving data between on-premises storage and Amazon S3 or Amazon EFS**.

With DataSync, you deploy a **DataSync agent** locally as a virtual machine to connect to your existing storage array or file system over the Network File Storage \(NFS\) protocol, and that agent sends and receives data to and from the fully managed DataSync service in AWS.

![](https://thor1345r75.s3.ap-south-1.amazonaws.com/migration+services/migration12.png)

### AWS Transfer for SFTP

AWS Transfer for SFTP is a fully managed service that enables the transfer of files directly into and out of Amazon S3 using the Secure File Transfer Protocol \(SFTP\)—also known as Secure Shell \(SSH\) File Transfer Protocol.

![](https://thor1345r75.s3.ap-south-1.amazonaws.com/migration+services/migration13.png)

