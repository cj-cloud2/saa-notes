# ---

**TITLE 7: Monitoring and Scaling (CloudWatch, ELB, Auto Scaling)**

## ---

**Question 26**

A company has decided to use AWS to achieve high availability. The company's architecture consists of an Application Load Balancer in front of an Auto Scaling group that consists of Amazon EC2 instances. The company uses Amazon CloudWatch metrics and alarms to monitor the architecture. A solutions architect notices that the company is not able to launch some instances. The solutions architect finds the following message: EC2 QUOTA EXCEEDED.

How can the solutions architect ensure that the company is able to launch all the EC2 instances correctly?

A. Modify the Auto Scaling group to raise the maximum number of instances that the company can launch.

B. Use Service Quotas to request an increase to the number of EC2 instances that the company can launch.

C. Recreate the Auto Scaling group to ensure the Auto Scaling group is connected to the Application Load Balancer.

D. Modify the CloudWatch metric that the company monitors to launch the instances.

**Correct Answer with Provided Explanation:**

**B. Correct.** The message "EC2 QUOTA EXCEEDED" means that the company has reached the current limit for a service. To launch more resources, the company must use Service Quotas to request a limit increase.

**Explanation for why other options are wrong:**

* **Option A:** Raising the "Maximum Capacity" within the Auto Scaling group settings allows the group to *attempt* to scale higher, but it cannot bypass the hard limits imposed on the AWS account level.  
* **Option C:** Recreating the Auto Scaling group does not change the account-level resource limits; the new group would still run into the same quota restriction.  
* **Option D:** Changing the metric only changes *when* a scale-out is triggered, but it does not fix the underlying issue that AWS is preventing the creation of new instances due to a limit.

## ---

**Question 41**

An environment has an Auto Scaling group across two Availability Zones referred to as AZ-a and AZ-b. AZ-a has four Amazon EC2 instances, and AZ-b has three EC2 instances. The Auto Scaling group uses a default termination policy. None of the instances are protected from a scale-in event.

How will Auto Scaling proceed if there is a scale-in event?

A. Auto Scaling selects an instance to terminate randomly.

B. Auto Scaling terminates the instance with the oldest launch configuration of all instances.

C. Auto Scaling selects the Availability Zone with four EC2 instances, and then continues to evaluate.

D. Auto Scaling terminates the instance with the closest next billing hour of all instances.

**Correct Answer with Provided Explanation:**

**C. Correct.** The default termination policy helps ensure that instances are distributed evenly across Availability Zones for high availability. This action is the starting point of the default termination policy if the Availability Zones have an unequal number of instances and the instances are unprotected.

**Explanation for why other options are wrong:**

* **Option A:** Auto Scaling follows a deterministic, multi-step logic to ensure balance and availability; it is never a purely random selection.

* **Option B:** While "oldest launch configuration" is a criterion used by the policy, it is only applied *after* the service has first identified which AZ has the most instances to maintain balance.  
* **Option D:** In previous years, billing hour proximity was a primary factor. However, modern termination policies prioritize AZ balance and launch template consistency over billing increments.

## ---

**Question 57**

A company hosts its website on AWS. To address the highly variable demand, the company has implemented Amazon EC2 Auto Scaling. Management is concerned that the company is over-provisioning its infrastructure, especially at the front end of the three-tier application. A solutions architect needs to ensure costs are optimized without impacting performance.

What should the solutions architect do to accomplish this?

A. Use Auto Scaling with Reserved Instances.

B. Use Auto Scaling with a scheduled scaling policy.

C. Use Auto Scaling with the suspend-resume feature.

D. Use Auto Scaling with a target tracking scaling policy.

**Correct Answer with Provided Explanation:**

**D. Correct.** A target tracking scaling policy creates and manages the Amazon CloudWatch alarms that invoke the scaling policy. A target tracking scaling policy also calculates the scaling adjustment based on the metric and the target value.

**Explanation for why other options are wrong:**

* **Option A:** Reserved Instances are a pricing model, not a scaling mechanism. They help with costs for baseline load but do not handle "highly variable demand" by adding or removing instances.  
* **Option B:** Scheduled scaling is used when traffic patterns are predictable (e.g., every Monday at 9 AM). For "highly variable" demand, scheduled scaling would still lead to over-provisioning or under-provisioning.  
* **Option C:** Suspend and resume stops and starts the scaling process itself; it does not provide a dynamic way to scale individual instances based on real-time load.

### ---

