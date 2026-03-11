# ---

**TITLE 9: Containers (ECS, EKS, Fargate)**

## ---

**Question 6**

A company has an application running as a service in Amazon Elastic Container Service (Amazon ECS) using the Amazon EC2 launch type. The application code makes AWS API calls to publish messages to Amazon Simple Queue Service (Amazon SQS).

What is the MOST secure method of giving the application permission to publish messages to Amazon SQS?

A. Use AWS Identity and Access Management (IAM) to grant SQS permissions to the role used by the launch configuration for the Auto Scaling group of the ECS cluster.

B. Create a new IAM user with SQS permissions. Then update the task definition to declare the access key ID and secret access key as environment variables.

C. Create a new IAM role with SQS permissions. Then update the task definition to use this role for the task role setting.

D. Update the security group used by the ECS cluster to allow access to Amazon SQS.

**Correct Answer with Provided Explanation:**

**C. Correct.** The creation of a new IAM role (based on the minimal Amazon Elastic Container Service Task Role template) ensures that only the containers that are created with this task will receive access to Amazon SQS.

**Explanation for why other options are wrong:**

* **Option A:** Assigning permissions to the EC2 instance role (the host) is insecure. Every container running on that host would inherit those permissions, even if they don't need SQS access, violating the principle of least privilege.  
* **Option B:** Using IAM User credentials (access keys) inside environment variables is a high security risk. Keys can be logged or viewed in the ECS console. Roles are the standard for secure, temporary credential management.  
* **Option D:** Security groups are network firewalls that control traffic based on IP addresses and ports. They have no ability to grant authorization for AWS service API calls like sqs:SendMessage.

## ---

**Question 37**

A solutions architect is designing a solution that involves orchestrating a series of Amazon Elastic Container Service (Amazon ECS) tasks. The tasks run on an ECS cluster that uses Amazon EC2 instances across multiple Availability Zones. The output and state data for all tasks must be stored. The amount of data that each task outputs is approximately 10 MB, and hundreds of tasks can run at a time. As old outputs are archived and deleted, the storage size will not exceed 1 TB.

Which storage solution should the solutions architect recommend to meet these requirements with the HIGHEST throughput?

A. An Amazon DynamoDB table that is accessible by all ECS cluster instances.

B. An Amazon Elastic Block Store (Amazon EBS) volume that is mounted to the ECS cluster instances.

C. An Amazon Elastic File System (Amazon EFS) file system with Bursting Throughput mode.

D. An Amazon Elastic File System (Amazon EFS) file system with Provisioned Throughput mode.

**Correct Answer with Provided Explanation:**

**D. Correct.** There are two throughput modes to choose from for your file system: Bursting Throughput mode and Provisioned Throughput mode. With Bursting Throughput mode, throughput on Amazon EFS scales as the size of your file system grows. With Provisioned Throughput mode, you can instantly provision the throughput of your file system (in MiB/s), independent of the amount of data stored. This solution provides the highest throughput without changing the size of the file system.

**Explanation for why other options are wrong:**

* **Option A:** DynamoDB is a NoSQL database. While high-performance, it is not a file system and would require significant application refactoring to store 10 MB output files as items or attributes.  
* **Option B:** Standard EBS volumes can only be attached to a single EC2 instance at a time. They cannot be shared across multiple instances in an ECS cluster to store combined task state.  
* **Option C:** Since the total storage size is relatively small (under 1 TB), Bursting Throughput mode would not provide high enough baseline performance for hundreds of concurrent tasks compared to Provisioned Throughput.

## ---

**Question 42**

A user is designing a new service that receives location updates from 3,600 rental cars every hour. The cars upload their location to an Amazon S3 bucket. Each location must be checked for distance from the original rental location.

Which services will process the updates and automatically scale?

A. Amazon EC2 and Amazon Elastic Block Store (Amazon EBS).

B. Amazon Data Firehose and Amazon S3.

C. Amazon Elastic Container Service (Amazon ECS) and Amazon RDS.

D. Amazon S3 events and AWS Lambda.

**Correct Answer with Provided Explanation:**

**D. Correct.** When an object is placed in an S3 bucket, that action needs to invoke an action that calculates the distance from the car to the original rental location. S3 Event Notifications calls the Lambda function, and Lambda runs the code to do the calculation.

**Explanation for why other options are wrong:**

* **Option A:** EC2 does not scale "automatically" without an Auto Scaling group and specific scaling policies. It is not an event-driven architecture by default.

* **Option B:** While Firehose can move data, it is not a compute service used for running distance calculation logic or processing individual S3 object uploads.  
* **Option C:** ECS can run the code, but it is not as seamlessly "automatic" or cost-effective for 3,600 small, intermittent hourly updates compared to the serverless nature of Lambda.

### ---

