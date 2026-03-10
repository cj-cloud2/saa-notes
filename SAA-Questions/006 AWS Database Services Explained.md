# ---

**TITLE 6: Databases (RDS, DynamoDB, Aurora)**

## ---

**Question 13**

An application running in a private subnet accesses an Amazon DynamoDB table. The data cannot leave the AWS network to meet security requirements.

How should this requirement be met?

A. Configure a network ACL on DynamoDB to limit traffic to the private subnet.

B. Enable DynamoDB encryption at rest using an AWS Key Management Service (AWS KMS) key.

C. Add a NAT gateway and configure the route table on the private subnet.

D. Create a VPC endpoint for DynamoDB and configure the endpoint policy.

**Correct Answer with Provided Explanation:**

**D. Correct.** With an endpoint inside the VPC and routing to that endpoint, traffic that is destined for DynamoDB would not leave the network.

**Explanation for why other options are wrong:**

* **Option A:** DynamoDB is a public-facing service and does not reside within your VPC subnets; therefore, traditional VPC Network ACLs cannot be applied to it.  
* **Option B:** Encryption at rest protects the data on the physical disk but has no impact on the network path the data takes between the application and the database.  
* **Option C:** A NAT Gateway sends traffic over the public internet to reach the public DynamoDB endpoint, which violates the requirement that data must not leave the AWS network.

## ---
**Question 15**

A company's legacy application is currently relying on a single-instance Amazon RDS MySQL database without encryption. Due to new compliance requirements, all existing and new data in this database must be encrypted.

How should this be accomplished?

A. Create an Amazon S3 bucket with server-side encryption turned on. Move all the data to Amazon S3. Delete the RDS instance.

B. Configure RDS Multi-AZ mode with encryption at rest turned on. Perform a failover to the standby instance to delete the original instance.

C. Take a snapshot of the RDS instance. Create an encrypted copy of the snapshot. Restore the RDS instance from the encrypted snapshot.

D. Create an RDS read replica with encryption at rest turned on. Promote the read replica to primary and switch the application over to the new primary. Delete the old RDS instance.

**Correct Answer with Provided Explanation:**

**C. Correct.** You can configure encryption for an RDS DB instance only when you create the DB instance. Alternatively, you can create a snapshot of the RDS instance. Next, you create an encrypted copy of the snapshot. Finally, you can create an RDS instance from the encrypted snapshot.

**Explanation for why other options are wrong:**

* **Option A:** Moving data to S3 completely changes the application architecture from relational to object storage, which is not a feasible solution for a legacy relational application.  
* **Option B:** Enabling Multi-AZ is a high-availability feature; it does not allow you to toggle encryption on an existing unencrypted database instance.  
* **Option D:** In RDS MySQL, you cannot create an encrypted read replica from an unencrypted primary instance. The encryption status of the replica must match the primary.

## ---

**Question 30**

A company needs to implement a relational database with a multi-Region disaster recovery Recovery Point Objective (RPO) of 1 second and a Recovery Time Objective (RTO) of 1 minute.

Which AWS solution can achieve this?

A. Amazon Aurora Global Database

B. Amazon DynamoDB global tables.

C. Amazon RDS for MySQL with Multi-AZ turned on.

D. Amazon RDS for MySQL with a cross-Region snapshot copy

**Correct Answer with Provided Explanation:**

**A. Correct.** This solution provides your application with an effective RPO of 1 second and an RTO of less than 1 minute.

**Explanation for why other options are wrong:**

* **Option B:** While DynamoDB global tables provide excellent RPO/RTO, the question specifically asks for a **relational** database. DynamoDB is NoSQL.  
* **Option C:** Multi-AZ provides high availability within a single Region, but it does not provide disaster recovery across multiple Regions.  
* **Option D:** Cross-Region snapshots are manual or scheduled and involve significant data transfer time; they cannot meet a 1-second RPO or a 1-minute RTO.

## ---

**Question 34**

A database architect is designing an online gaming application that uses a simple, unstructured data format. The database must have the ability to store user information and to track the progress of each user. The database must have the ability to scale to millions of users throughout the week.

Which database service will meet these requirements with the LEAST amount of operational support?

A. Amazon RDS Multi-AZ

B. Amazon Neptune.

C. Amazon DynamoDB

D. Amazon Aurora

**Correct Answer with Provided Explanation:**

**C. Correct.** DynamoDB is a fast, flexible NoSQL database service for single-digit millisecond performance at any scale. DynamoDB can support game platforms with user data, session history, and leaderboards for millions of concurrent users.

**Explanation for why other options are wrong:**

* **Option A:** RDS requires more management (patching, scaling) and is designed for structured relational data, which doesn't fit the "unstructured" requirement as naturally as NoSQL.  
* **Option B:** Neptune is a graph database; while it handles unstructured data, it is specifically for highly connected datasets (like social networks) rather than simple user progress tracking.  
* **Option D:** Aurora is a high-performance relational database, but it still requires more operational overhead for scaling compared to the seamless, serverless scaling of DynamoDB.

## ---

**Question 39**

A company has a three-tier architecture solution in which an application writes to a relational database. Because of frequent requests, the company wants to cache data whenever the application writes data to the database. The company's priority is to minimize latency for data retrieval and to ensure that data in the cache is never stale.

Which caching strategy should the company use to meet these requirements?

A. Amazon ElastiCache with write-through

B. Amazon DynamoDB Accelerator (DAX).

C. Amazon ElastiCache with lazy loading.

D. Amazon Simple Queue Service (Amazon SQS)

**Correct Answer with Provided Explanation:**

**A. Correct.** The write-through strategy adds data or updates data that is in the cache whenever the application writes data to the database. Data in the cache is never stale because the data in the cache is updated every time the application writes the data to the database. ElastiCache supports relational databases.

**Explanation for why other options are wrong:**

* **Option B:** DAX is a dedicated cache specifically for DynamoDB. It cannot be used to cache a relational database like RDS.  
* **Option C:** Lazy loading only caches data after a "cache miss." This means the cache can contain stale data if the underlying database is updated without the cache being refreshed.  
* **Option D:** SQS is a messaging queue used for decoupling services; it is not a caching mechanism for low-latency data retrieval.

## ---

**Question 44**

A company is using Amazon DynamoDB to stage its product catalog, which is 1 TB in size. A product entry consists of an average of 100 KB of data, and the average traffic is about 250 requests each second. A database administrator has provisioned 3,000 read capacity units (RCUs) of throughput.

However, some products are popular among users. Users are experiencing delays or timeouts because of throttling. The popularity is expected to continue to increase, but the number of products will stay constant.

What should a solutions architect do as a long-term solution to this problem?

A. Increase the provisioned throughput to 6,000 RCUs.

B. Use DynamoDB Accelerator (DAX) to maintain the frequently read items.

C. Augment DynamoDB by storing only the key product attributes, with the details stored in Amazon S3.

D. Change the partition key to consist of a hash of the product key and product type instead of just the product key.

**Correct Answer with Provided Explanation:**

**B. Correct.** DAX provides increased throughput for read-heavy workloads. DAX also provides potential cost savings by reducing the need to overprovision RCUs. This feature is especially beneficial for applications that require repeated reads for popular products.

**Explanation for why other options are wrong:**

* **Option A:** Increasing RCUs is a "brute force" approach that significantly increases cost without solving the "hot partition" problem created by popular items.

* **Option C:** While this reduces the item size in DynamoDB, it adds complexity and doesn't directly address the throttling caused by high-frequency reads of popular items.  
* **Option D:** Changing the partition key helps with write distribution but doesn't solve the read throttling issue for specific popular items that users are requesting by their unique ID.

## ---

**Question 61**

A solutions architect finds that an Amazon Aurora cluster with On-Demand Instance pricing is being underutilized for a blog application. The application is used only for a few minutes several times each day for reads.

What should a solutions architect do to optimize utilization MOST cost-effectively?

A. Turn on Auto Scaling on the original Aurora database.

B. Refactor the blog application to use Aurora parallel query.

C. Convert the original Aurora database to an Aurora global database.

D. Convert the original Aurora database to Amazon Aurora Serverless.

**Correct Answer with Provided Explanation:**

**D. Correct.** If you use Aurora Serverless, you pay for only the database resources that you consume on a per-second basis.

**Explanation for why other options are wrong:**

* **Option A:** Aurora Auto Scaling adds or removes Read Replicas. It does not allow the primary instance to scale down to zero or scale its capacity to match very brief bursts of activity.  
* **Option B:** Parallel query is used to speed up analytical queries on large datasets; it does not help with cost optimization for underutilized instances.  
* **Option C:** Global Database is for cross-region disaster recovery and low-latency global reads; it increases costs rather than reducing them for an underutilized app.

### ---

