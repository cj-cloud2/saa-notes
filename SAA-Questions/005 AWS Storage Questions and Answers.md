# ---

**TITLE 5: Storage (S3, EBS, EFS)**

## ---

**Question 14**

A company is planning to use Amazon S3 to store images uploaded by its users. The images must be encrypted at rest in Amazon S3. The company does not want to spend time managing and rotating the keys, but it does want to control who can access those keys.

What should a solutions architect use to accomplish this?

A. Server-Side Encryption with encryption keys stored in an S3 bucket.

B. Server-Side Encryption with Customer-Provided Keys (SSE-C).

C. Server-Side Encryption with encryption keys stored in AWS Systems Manager Parameter Store.

D. Server-Side Encryption with AWS KMS-Managed Keys (SSE-KMS)

**Correct Answer with Provided Explanation:**

**D. Correct.** SSE-KMS uses AWS Key Management Service (AWS KMS) to create, manage, rotate, and control access to encryption keys.

**Explanation for why other options are wrong:**

* **Option A:** S3 does not offer a feature where keys are stored as standard objects in a bucket for server-side encryption purposes; this is structurally incorrect.  
* **Option B:** SSE-C requires the company to manage the keys themselves (storage, rotation, and tracking), which violates the requirement to avoid "managing and rotating the keys."  
* **Option C:** While Parameter Store can store strings, it is not the native or integrated solution for S3 server-side encryption; AWS KMS is the purpose-built service for this.

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

**Question 20**

A company is designing a website that will be hosted on Amazon S3.

How should users be prevented from linking directly to the assets in the S3 bucket?

A. Create a static website, then update the bucket policy to require users to access the resources with the static website URL.

B. Create an Amazon CloudFront distribution with an origin access control (OAC) and update the bucket policy to grant permission to the OAC only.

C. Create a static website, then configure an Amazon Route 53 record set with an alias pointing to the static website. Provide this URL to users.

D. Create an Amazon CloudFront distribution with an AWS WAF web ACL that permits access to the origin server through the distribution only.

**Correct Answer with Provided Explanation:**

**B. Correct.** To restrict access to the assets in S3 buckets, you can create an OAC and associate it with the website distribution. You would configure the S3 bucket permissions so that CloudFront can use the OAC to access the files in the S3 bucket and serve the files to the users. Afterward, the users can access the static website files only through CloudFront, not directly from the S3 bucket.

**Explanation for why other options are wrong:**

* **Option A:** S3 static website endpoints do not provide a mechanism to restrict access based on the "static website URL" in a way that prevents direct object URL access.

* **Option B:** Route 53 just provides a DNS alias; it does not provide any security or access control to the underlying S3 objects.  
* **Option D:** While WAF can protect CloudFront, it doesn't prevent a user from bypassing CloudFront and going directly to the S3 bucket URL unless an OAC/OAI is also used.

## ---

**Question 38**

A company is building a document storage application on AWS. The application runs on Amazon EC2 instances in multiple Availability Zones. The company requires the document store to be highly available. The documents need to be available to all EC2 instances hosting the application and returned immediately when requested multiple times per month. The lead engineer has configured the application to use Amazon Elastic Block Store (Amazon EBS) to store the documents, but is willing to consider other options to meet the availability requirement.

What should a solutions architect recommend?

A. Snapshot the EBS volumes regularly and build new volumes using those snapshots in additional Availability Zones.

B. Use Amazon EBS for the EC2 instance root volumes. Configure the application to build the document store on Amazon S3 Standard.

C. Use Amazon EBS for the EC2 instance root volumes. Configure the application to build the document store on Amazon S3 Glacier Flexible Retrieval.

D. Use at least three Provisioned IOPS EBS volumes for EC2 instances. Mount the volumes to the EC2 instances in a RAID 5 configuration.

**Correct Answer with Provided Explanation:**

**B. Correct.** This solution addresses the data availability requirement by using S3 Standard. This solution also follows best practices for data storage design. Because Amazon S3 is a Regional service, the document store will be highly available and accessible to EC2 instances across Availability Zones. The application will return documents immediately upon request.

**Explanation for why other options are wrong:**

* **Option A:** Snapshots are point-in-time; this would lead to data inconsistency across AZs as new documents wouldn't be immediately available in all zones.  
* **Option C:** Glacier is for long-term archiving with retrieval times ranging from minutes to hours. It does not meet the requirement for documents to be "returned immediately."  
* **Option D:** Standard EBS volumes cannot be natively mounted to multiple instances across different AZs simultaneously, and RAID 5 adds significant complexity without solving the cross-AZ availability issue.

## ---

**Question 43**

An application provides a feature that allows users to securely download private and personal files. The web server is currently overwhelmed with serving files for download. A solutions architect must find a more effective solution to reduce the web server load and cost, and must allow users to download only their own files.

Which solution meets all requirements?

A. Store the files securely on Amazon S3 and have the application generate an Amazon S3 presigned URL for the user to download.

B. Store the files in an encrypted Amazon Elastic Block Store (Amazon EBS) volume, and use a separate set of servers to serve the downloads.

C. Have the application encrypt the files and store them in the local Amazon EC2 instance store prior to serving them up for download.

D. Create an Amazon CloudFront distribution to distribute and cache the files.

**Correct Answer with Provided Explanation:**

**A. Correct.** You can use presigned URLs to share access to your S3 buckets. When you create a presigned URL, you associate it with a specific action and an expiration date. Anyone who has access to the URL can perform the action embedded in the URL as if they were the original signing user.

**Explanation for why other options are wrong:**

* **Option B:** This still requires managing servers and scaling them, which does not effectively reduce the administrative load or cost compared to a serverless S3 solution.

* **Option C:** Instance stores are ephemeral; if the instance stops, the files are lost. Furthermore, this still keeps the download load on the web servers.  
* **Option D:** CloudFront is excellent for public or shared content, but managing individual private file access for thousands of users via CloudFront is more complex than simple S3 presigned URLs.

## ---

**Question 47**

A company runs an online media site, hosted on-premises. An employee posted a product review that contained videos and pictures. The review went viral, and the company needs to handle the resulting spike in website traffic.

What action would provide an immediate solution?

A. Redesign the website to use Amazon API Gateway, and use AWS Lambda to deliver content.

B. Add server instances using Amazon EC2 and use Amazon Route 53 with a failover routing policy.

C. Serve the images and videos using an Amazon CloudFront distribution created using the media site as the origin.

D. Use Amazon ElastiCache for Redis for caching and reducing the load requests from the origin.

**Correct Answer with Provided Explanation:**

**C. Correct.** The review contains videos and pictures, so the company can reduce the load by offloading the content serving from the servers to a content delivery network (CDN) such as CloudFront.

**Explanation for why other options are wrong:**

* **Option A:** Redesigning an entire website to use API Gateway and Lambda is a long-term project and cannot provide an "immediate solution" to a viral traffic spike.  
* **Option B:** Provisioning and configuring a fleet of EC2 instances and setting up failover is complex and slower than simply pointing a CDN at the existing site.  
* **Option D:** ElastiCache helps with database and application session load, but it doesn't solve the problem of high bandwidth consumption for large video and image files.

## ---

**Question 52**

A solutions architect at an ecommerce company wants to store application log data using Amazon S3. The solutions architect is unsure how frequently the logs will be accessed or which logs will be accessed the most. The company wants to keep costs as low as possible by using the appropriate S3 storage class.

Which S3 storage class should be implemented to meet these requirements?

A. S3 Glacier Flexible Retrieval (formerly S3 Glacier)

B. S3 Intelligent-Tiering

C. S3 Standard-Infrequent Access (S3 Standard-IA)

D. S3 One Zone-Infrequent Access (S3 One Zone-IA)

**Correct Answer with Provided Explanation:**

**B. Correct.** S3 Intelligent-Tiering provides automatic cost savings for data with unknown or variable access patterns without retrieval fees, performance impact, or operational overhead by automatically moving data to the most cost-effective access tier based on access frequency.

**Explanation for why other options are wrong:**

* **Option A:** Glacier is for data that is rarely accessed and requires minutes or hours to retrieve; it is not suitable for logs that might need immediate analysis.

* **Option C:** Standard-IA is for data that is accessed infrequently but requires rapid access. If the logs *are* accessed frequently, this class would incur high retrieval fees.

* **Option D:** One Zone-IA stores data in a single AZ, which provides lower durability/availability and is not recommended for critical application logs where the access pattern is unknown.

### ---

