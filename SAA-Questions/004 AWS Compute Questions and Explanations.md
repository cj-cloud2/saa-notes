# ---

**TITLE 4: Compute (EC2, Lambda, ECS, Auto Scaling)**

## ---

**Question 12**

An application runs on Amazon EC2 instances in multiple Availability Zones behind an Application Load Balancer. The load balancer is in public subnets. The EC2 instances are in private subnets and must not be accessible from the internet. The EC2 instances must call external services on the internet. Each Availability Zone must be able to call the external services, regardless of the status of the other Availability Zones.

How should these requirements be met?

A. Create a NAT gateway attached to the VPC. Add a route to the gateway that connects to each private subnet route table.

B. Configure an internet gateway. Add a route to the gateway that connects to each private subnet route table.

C. Create a NAT instance in the private subnet of each Availability Zone. Update the route tables for each private subnet to direct internet-bound traffic to the NAT instance.

D. Create a NAT gateway in each Availability Zone. Update the route tables for each private subnet to direct internet-bound traffic to the NAT gateway.

**Correct Answer with Provided Explanation:**

**D. Correct.** A NAT gateway is assigned to a public subnet and is associated with a single Availability Zone. This solution ensures that each Availability Zone is independent of the others for internet routing.

**Explanation for why other options are wrong:**

* **Option A:** Using a single NAT gateway creates a single point of failure. If the Availability Zone containing that NAT gateway fails, all other AZs lose their ability to reach the internet.  
* **Option B:** Instances in a private subnet cannot use an Internet Gateway directly; they require a NAT device to translate private IPs to public IPs for internet communication.  
* **Option C:** NAT instances are not managed services; they require manual scaling, patching, and high-availability configuration, leading to higher operational overhead compared to NAT gateways.

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

* **Option A:** Modifying the Auto Scaling group settings (Desired/Max capacity) does not override the account-level regional limits set by AWS.  
* **Option C:** Recreating the group does not address the underlying issue of the account-level resource limit being reached.  
* **Option D:** Changing the monitoring metric might stop the scaling trigger, but it doesn't solve the problem of needing more capacity than the current quota allows.

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

* **Option A:** Termination is not random; it follows a specific multi-step logic to maintain high availability and balance across zones.

* **Option B:** While "oldest launch configuration" is a step in the policy, it is only evaluated *after* Auto Scaling has identified which Availability Zone has the most instances.  
* **Option D:** Billing hour proximity was a factor in older versions of the policy but is no longer the primary driver for modern EC2 Auto Scaling termination logic.

## ---

**Question 59**

A development team is deploying a new product on AWS and is using AWS Lambda as part of the deployment. The team allocates 512 MB of memory for one of the Lambda functions. With this memory allocation, the function is completed in 2 minutes. The function runs millions of times monthly, and the development team is concerned about cost. The team conducts tests to see how different Lambda memory allocations affect the cost of the function.

Which steps will reduce the Lambda costs for the product? (Select TWO.)

A. Increase the memory allocation for this Lambda function to 1,024 MB if this change causes the run time of each function to be less than 1 minute.

B. Increase the memory allocation for this Lambda function to 1,024 MB if this change causes the run time of each function to be less than 90 seconds.

C. Reduce the memory allocation for this Lambda function to 256 MB if this change causes the run time of each function to be less than 4 minutes.

D. Increase the memory allocation for this Lambda function to 2,048 MB if this change causes the run time of each function to be less than 1 minute.

E. Reduce the memory allocation for this Lambda function to 256 MB if this change causes the run time of each function to be less than 5 minutes.

**Correct Answer with Provided Explanation:**

**A and C are Correct.**

**A. Correct.** In this case, the team increases the memory allocation by 100%. The new run time is more than 100% faster (less than 1 minute), so a reduction in the overall costs will occur.

**C. Correct.** In this case, the team reduces the memory by 50%. The new run time is less than 200% slower (less than 4 minutes), so a reduction in the overall costs will occur.

**Explanation for why other options are wrong:**

* **Option B:** If the run time is 90 seconds (a 25% reduction in time) but memory is doubled (100% increase in cost per second), the total cost per execution increases.  
* **Option D:** Increasing memory to 2,048 MB (4x increase) and only getting a 50% reduction in time (1 minute) results in a much higher total cost.  
* **Option E:** If memory is halved (50% cost) but the time more than doubles (from 2 mins to 5 mins), the total cost per execution increases.

### ---

