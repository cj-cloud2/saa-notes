# ---

**TITLE 14: Analytics, Specialized Services, and Cost Optimization**

## ---

**Question 17**

A company's cloud operations team wants to standardize resource remediation. The company wants to provide a standard set of governance evaluations and remediations to all member accounts in its organization in AWS Organizations.

Which self-managed AWS service can the company use to meet these requirements with the LEAST amount of operational effort?

A. AWS Security Hub compliance standards

B. AWS Config conformance packs

C. AWS CloudTrail

D. AWS Trusted Advisor

**Correct Answer with Provided Explanation:**

**B. Correct.** AWS Config conformance packs are collections of AWS Config rules and remediation actions that you can deploy as a single entity in an account and a Region or across an organization in AWS Organizations.

**Explanation for why other options are wrong:**

* **Option A:** Security Hub provides a central view of security alerts but doesn't natively package custom remediation sets for deployment across an organization as efficiently as conformance packs.  
* **Option B:** CloudTrail is a logging service for API calls; it does not perform governance evaluation or resource remediation.  
* **Option D:** Trusted Advisor provides recommendations for cost, security, and performance, but it does not allow for the creation and deployment of standardized remediation packages.

## ---

**Question 19**

An application running on AWS Lambda requires an API key to access a third-party service. The key must be stored securely with audited access to the Lambda function only.

What is the MOST secure way to store the key?

A. As an object in Amazon S3

B. As a secure string in AWS Systems Manager Parameter Store

C. Inside a file on an Amazon EBS volume attached to the Lambda function

D. Inside a secrets file stored on Amazon EFS

**Correct Answer with Provided Explanation:**

**B. Correct.** Parameter Store provides secure, hierarchical storage for configuration data management and secrets management. You can store data such as passwords, database strings, Amazon Machine Image (AMI) IDs, and license codes as parameter values. You can store values as plaintext or encrypted data.

**Explanation for why other options are wrong:**

* **Option A:** While S3 can be encrypted, it is not a dedicated secrets management tool and requires more complex IAM and bucket policy management to audit access specifically for a single Lambda function compared to Parameter Store.  
* **Option C:** Lambda is serverless; while you can now attach EBS (via storage mounts), it is not the standard or most secure way to manage API keys.  
* **Option D:** Similar to EBS, EFS is a file system. Managing a secrets file manually on a file system is less secure and harder to audit than using a managed service like Parameter Store or Secrets Manager.

## ---

**Question 50**

A company wants to measure the effectiveness of its recent marketing campaigns. The company performs batch processing on .csv files of sales data and stores the results in an Amazon S3 bucket once every hour. The S3 bucket contains petabytes of objects. The company runs one-time queries in Amazon Athena to determine which products are most popular on a particular date for a particular region. Queries sometimes fail or take longer than expected to finish running.

Which actions should a solutions architect take to improve the query performance and reliability? (Select TWO.)

A. Reduce the S3 object sizes to less than 128 MB.

B. Partition the data by date and region in Amazon S3.

C. Store the files as large, single objects in Amazon S3.

D. Use Amazon Kinesis Data Analytics to run the queries as part of the batch processing operation.

E. Use an AWS Glue extract, transform, and load (ETL) process to convert the .csv files into Apache Parquet format.

**Correct Answer with Provided Explanation:**

**B and E are Correct.**

**B. Correct.** Partitions divide your table into parts and keep the related data together based on column values such as date, country, or Region. Partitions can help reduce the amount of data that Athena scans each query, which improves performance.

**E. Correct.** Apache Parquet is a columnar data store. You can reduce the number of bytes that Athena reads from Amazon S3 by achieving better compression ratios or by skipping blocks of data.

**Explanation for why other options are wrong:**

* **Option A:** Reducing file sizes too much (fragmentation) can actually slow down Athena due to the overhead of opening too many small files.  
* **Option C:** Storing data as single massive objects prevents Athena from reading data in parallel, which significantly degrades performance.  
* **Option D:** Kinesis Data Analytics is for real-time streaming data, not for querying historical batch data stored in S3.

## ---

**Question 51**

A company wants to create an audio version of its product manual. The product manual contains custom product names and abbreviations. The product manual is divided into sections.

Which solution will meet these requirements with the LEAST operational overhead?

A. Use Amazon Polly. Build custom lexicons for the product names and abbreviations. Use the StartSpeechSynthesisTask API operation for each section of the product manual.

B. Use Amazon Polly. Build custom Speech Synthesis Markup Language (SSML) for the product names and abbreviations. Use the StartDocumentTextDetection API operation for each section of the product manual.

C. Use Amazon Textract. Build custom Speech Synthesis Markup Language (SSML) for the product names and abbreviations. Use the StartDocumentTextDetection API operation for each section of the product manual.

D. Use Amazon Textract. Build custom lexicons for the product names and abbreviations. Use the StartTranscriptionJob API operation for each section of the product manual.

**Correct Answer with Provided Explanation:**

**A. Correct.** Amazon Polly converts text into life-like speech. Your text might include an acronym, such as "W3C." You can use a lexicon to define an alias for those words. Lexicons are appropriate for custom vocabulary that you use in product manuals.

**Explanation for why other options are wrong:**

* **Option B:** While SSML can handle pronunciations, managing lexicons is more efficient for "custom product names" across a whole manual. Furthermore, StartDocumentTextDetection is a Textract API, not a Polly API.  
* **Option C:** Amazon Textract is used for OCR (extracting text from images/PDFs), not for converting text to speech.  
* **Option D:** Textract does not have a "StartTranscriptionJob"; that is an Amazon Transcribe API (used for speech-to-text).

## ---

**Question 54**

A company runs an application on three very large Amazon EC2 instances in a single Availability Zone in the us-east-1 Region. Multiple 16 TB Amazon Elastic Block Store (Amazon EBS) volumes are attached to each EC2 instance. The operations team uses an AWS Lambda script to stop the instances on evenings and weekends, and start the instances on weekday mornings.

When looking at monthly Cost Explorer charges, the overall charges are higher than the estimate.

What is the MOST likely cost factor that the company overlooked?

A. EC2 data transfer charges between the instances are much higher than expected.

B. EC2 and EBS rates are higher in us-east-1 than most other AWS Regions.

C. The Lambda charges to stop and start the instances are much higher than expected.

D. The company is being billed for the EBS storage on nights and weekends.

**Correct Answer with Provided Explanation:**

**D. Correct.** When you stop an EC2 instance, AWS does not charge you for usage or data transfer. However, AWS does charge you for the storage of any EBS volumes that are attached to the instance.

**Explanation for why other options are wrong:**

* **Option A:** If instances are stopped, they aren't transferring data. Even when running, intra-AZ data transfer is generally free.  
* **Option B:** us-east-1 is typically one of the most cost-effective regions in the AWS network.  
* **Option C:** A Lambda script running twice a day is extremely inexpensive and would not significantly impact a monthly bill for "very large" instances and 16 TB volumes.

## ---

**Question 58**

Cost Explorer is showing charges higher than expected for Amazon EBS volumes. A significant portion of the charges are from Provisioned IOPS SSD (io2) volume types. Controlling costs is the highest priority.

Which steps should the user take to analyze and reduce the EBS costs without incurring any application downtime? (Select TWO.)

A. Use the Amazon EC2 ModifyInstanceAttribute action to enable EBS optimization on the application server instances.

B. Use the Amazon CloudWatch GetMetricData action to evaluate the read/write operations and read/write bytes of each volume.

C. Use the Amazon EC2 ModifyVolume action to reduce the size of the underutilized io2 volumes.

D. Use the Amazon EC2 ModifyVolume action to change the volume type of the underutilized io2 volumes to General Purpose SSD (gp3).

E. Use an Amazon S3 PutBucketPolicy action to migrate existing volume snapshots to Amazon S3 Glacier Flexible Retrieval.

**Correct Answer with Provided Explanation:**

**B and D are Correct.**

**B. Correct.** The CloudWatch GetMetricData action can show the IOPS and throughput of an io2 volume to help you determine if the io2 volume is a good candidate for modification.

**D. Correct.** You can make a change with the EC2 ModifyVolume action without incurring any volume downtime. Change from io2 to gp3 to reduce costs.

**Explanation for why other options are wrong:**

* **Option A:** Enabling EBS optimization doesn't reduce the cost of the volumes themselves; it just improves the network throughput between the instance and the volume.  
* **Option C:** You cannot reduce the size of an EBS volume once it has been provisioned. You can only increase it or create a new, smaller volume and migrate the data.  
* **Option E:** Snapshots are already stored in S3. Moving them to Glacier can save money on backups, but the question is specifically about the high cost of active io2 volumes.

### ---

