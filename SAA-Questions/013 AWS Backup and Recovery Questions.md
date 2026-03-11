# ---

**TITLE 13: Backup and Recovery (AWS Backup, Multi-AZ, Cross-Region Replication)**

## ---

**Question 30**

A company needs to implement a relational database with a multi-Region disaster recovery Recovery Point Objective (RPO) of 1 second and a Recovery Time Objective (RTO) of 1 minute.

Which AWS solution can achieve this?

A. Amazon Aurora Global Database

B. Amazon DynamoDB global tables.

C. Amazon RDS for MySQL with Multi-AZ turned on.

D. Amazon RDS for MySQL with a cross-Region snapshot copy.

**Correct Answer with Provided Explanation:**

**A. Correct.** This solution provides your application with an effective RPO of 1 second and an RTO of less than 1 minute.

**Explanation for why other options are wrong:**

* **Option B:** While DynamoDB global tables offer sub-second replication, they are a NoSQL service. The requirement specifically identifies a **relational** database.  
* **Option C:** Multi-AZ provides high availability and synchronous replication within a *single* Region. It does not provide the multi-Region disaster recovery capability requested.  
* **Option D:** Snapshots are point-in-time backups. Even with cross-Region copying, the process of taking, moving, and restoring a snapshot takes significantly longer than 1 minute (RTO) and results in much more than 1 second of data loss (RPO).

## ---

**Question 33**

A solutions architect is responsible for a new highly available three-tier architecture on AWS. An Application Load Balancer distributes traffic to two different Availability Zones with an auto scaling group that consists of Amazon EC2 instances and a Multi-AZ Amazon RDS DB instance. The solutions architect must recommend a multi-Region recovery plan with a recovery time objective (RTO) of 30 minutes. Because of budget constraints, the solutions architect cannot recommend a plan that replicates the entire architecture. The recovery plan should not use the secondary Region unless necessary.

Which disaster recovery strategy will meet these requirements?

A. Backup and restore.

B. Multi-site active/active

C. Pilot light

D. Warm standby.

**Correct Answer with Provided Explanation:**

**C. Correct.** A pilot light strategy meets all the requirements. This strategy does not have a large increase in cost. This strategy offers an RTO within 10s of minutes.

**Explanation for why other options are wrong:**

* **Option A:** "Backup and restore" is the most cost-effective but typically has an RTO measured in hours, as you must provision all infrastructure and restore all data from scratch, which exceeds the 30-minute requirement.  
* **Option B:** "Multi-site active/active" replicates the entire architecture and serves traffic from both regions simultaneously. This is the most expensive option and violates the budget constraint.  
* **Option D:** "Warm standby" keeps a scaled-down but functional version of the entire environment running in the secondary region. While it has a lower RTO, it is more expensive than "Pilot light," which only keeps the data/databases live.

## ---

**Question 36**

A team has an application that detects when new objects are uploaded into an Amazon S3 bucket. The uploads invoke an AWS Lambda function to write object metadata into an Amazon DynamoDB table and an Amazon RDS for PostgreSQL database.

Which action should the team take to ensure high availability?

A. Enable Cross-Region Replication in the S3 bucket.

B. Create a Lambda function for each Availability Zone the application is deployed in.

C. Enable Multi-AZ on the RDS for PostgreSQL database.

D. Create a DynamoDB stream for the DynamoDB table.

**Correct Answer with Provided Explanation:**

**C. Correct.** By default, Amazon RDS is deployed to a single Availability Zone. Multi-AZ is the standard option to provide high availability. In a Multi-AZ setup, RDS DB instances are synchronously replicated in other Availability Zones to provide high availability and failover support.

**Explanation for why other options are wrong:**

* **Option A:** Cross-Region Replication (CRR) is a disaster recovery strategy for regional failure; it does not address the high availability of the database tier within a single region.  
* **Option B:** Lambda is a regional service. AWS automatically runs Lambda in multiple Availability Zones to ensure high availability; you do not need to create separate functions per AZ manually.  
* **Option D:** DynamoDB streams are used to capture changes to items in the table for downstream processing. While useful for integration, they do not impact the high availability of the RDS database.

### ---

