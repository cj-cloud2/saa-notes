# ---

**TITLE 12: Edge Services (Route 53, CloudFront, Shield, WAF, Outposts)**

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

* **Option A:** Updating a bucket policy to a "URL" is not a valid security constraint in S3; bucket policies use Principals and Conditions (like aws:Referer), but OAC is the standard, more secure method.

* **Option C:** Route 53 is a DNS service. Providing an alias URL does not physically block someone from using the original S3 bucket URL if they know it.  
* **Option D:** AWS WAF can filter traffic at the CloudFront level, but it does not prevent direct access to the S3 bucket origin itself unless OAC is implemented to lock down the bucket.

## ---

**Question 29**

A company runs a website on Amazon EC2 instances behind an ELB Application Load Balancer. Amazon Route 53 is used for the DNS. The company wants to set up a backup website with a message including a phone number and email address that users can reach if the primary website is down.

How should the company deploy this solution?

A. Use Amazon S3 website hosting for the backup website and a Route 53 failover routing policy.

B. Use Amazon S3 website hosting for the backup website and a Route 53 latency routing policy.

C. Deploy the application in another AWS Region and use ELB health checks for failover routing.

D. Deploy the application in another AWS Region and use server-side redirection on the primary website.

**Correct Answer with Provided Explanation:**

**A. Correct.** Because the backup website uses static data, Amazon S3 is an ideal solution. Additionally, a Route 53 failover policy will direct users to the S3 hosted website only if the primary ELB based endpoint is unavailable.

**Explanation for why other options are wrong:**

* **Option B:** Latency routing directs users to the region with the lowest latency; it does not provide a "backup" or "maintenance" page functionality when the primary site fails.  
* **Option C:** This involves replicating the entire application stack in a second region, which is expensive and unnecessary for displaying a simple contact message.  
* **Option D:** If the primary website is down, server-side redirection will not work because the servers themselves are unavailable to process the redirect.

## ---

**Question 32**

A company wants to build an immutable infrastructure for its software applications. The company wants to test the software applications before sending traffic to them. The company seeks an efficient solution that limits the effects of application bugs.

Which combination of steps should a solutions architect recommend? (Select TWO.)

A. Use AWS CloudFormation to update the production infrastructure and roll back the stack if the update fails.

B. Apply Amazon Route 53 weighted routing to test the staging environment and gradually increase the traffic as the tests pass.

C. Apply Amazon Route 53 failover routing to test the staging environment and fail over to the production environment if the tests pass.

D. Use AWS CloudFormation with a parameter set to the staging value in a separate environment other than the production environment.

E. Use AWS CloudFormation to deploy the staging environment with a snapshot deletion policy and reuse the resources in the production environment if the tests pass.

**Correct Answer with Provided Explanation:**

**B and D are Correct.**

**B. Correct.** Route 53 weighted routing gives you the ability to send a percentage of traffic to multiple resources. You can use a blue/green deployment strategy to deploy software applications predictably and to quickly roll back deployments if tests fail.

**D. Correct.** The company could use a separate environment to test changes before the company deploys changes to production.

**Explanation for why other options are wrong:**

* **Option A:** Rolling back a production stack happens *after* the update has already begun affecting production users; it is not a pre-traffic testing strategy.  
* **Option C:** Failover routing is an "all-or-nothing" switch based on health checks, making it unsuitable for the "gradual" traffic testing required for blue/green deployments.  
* **Option E:** Reusing resources between staging and production violates the "immutable" principle where each environment is built from scratch and discarded rather than modified or moved.

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

* **Option A:** Redesigning a site to be serverless is a major architectural overhaul that takes weeks or months; it is not an "immediate" solution for a viral spike.  
* **Option B:** Adding EC2 instances requires migrating the application to AWS and setting up synchronization, which is too slow for an immediate traffic crisis.  
* **Option C:** Pointing CloudFront at an existing on-premises origin can be done in minutes and immediately offloads the heaviest traffic (images/videos).  
* **Option D:** ElastiCache helps with database queries and session data, but it does not solve the problem of high bandwidth consumption from large media files.

### ---

