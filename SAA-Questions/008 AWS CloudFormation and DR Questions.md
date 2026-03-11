# ---

**TITLE 8: Automation (CloudFormation & Standardized Remediation)**

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

* **Option A:** Rolling back a production stack after a failure is reactive; it doesn't meet the requirement of testing "before" sending traffic to the applications.  
* **Option C:** Failover routing is for Disaster Recovery (Primary/Secondary), not for the gradual testing of a new staging environment (Blue/Green).  
* **Option E:** Reusing resources from staging in production contradicts the principle of "immutable infrastructure," where environments are distinct and replaced rather than modified or moved.

## ---

**Question 63**

A company has a well-architected application that streams audio data by using UDP in the AWS Cloud. The company hosts the application in the eu-central-1 Region. The company plans to offer services to North American users. A solutions architect must improve application network performance for the North American users.

Which of the following is the MOST cost-effective solution?

A. Create an AWS Global Accelerator standard accelerator with an endpoint group in eu-central-1.

B. Use AWS CloudFormation to deploy additional application infrastructure in the us-east-1 Region and the us-west-1 Region.

C. Create an Amazon CloudFront distribution and use the North America (United States, Mexico, Canada) and Europe and Israel price classes.

D. Configure the application to use an Amazon Route 53 latency-based routing policy.

**Correct Answer with Provided Explanation:**

**A. Correct.** Global Accelerator improves TCP and UDP network performance for users around the world, even if a company hosts an application in only one Region. Global Accelerator routes application traffic to the nearest Global Accelerator edge location. Application traffic travels over the well-monitored, congestion-free, and redundant AWS global network to the endpoint. Global Accelerator optimizes the network path by maximizing the time that traffic is on the AWS network.

**Explanation for why other options are wrong:**

* **Option B:** While using CloudFormation to deploy to new regions works, it is significantly more expensive and operationally complex than using a single-region backend with an accelerator.  
* **Option C:** CloudFront is designed for HTTP/HTTPS traffic and does not support the UDP protocol used by this specific audio streaming application.  
* **Option D:** Latency-based routing only works if there are endpoints in multiple regions to route to. Without deploying extra infrastructure (as in Option B), this policy has no effect.

### ---

