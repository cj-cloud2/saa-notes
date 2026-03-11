# ---

**TITLE 10: Networking Part 2 (VPC Peering, Transit Gateway, Direct Connect, VPC Endpoints)**

## ---

**Question 1**

A company wants to create an application that will transmit protected health information (PHI) to thousands of service consumers in different AWS accounts. The application servers will sit in private VPC subnets. The routing for the application must be fault tolerant.

What should be done to meet these requirements?

A. Create a VPC endpoint service and grant permissions to specific service consumers to create a connection.

B. Create a virtual private gateway connection between each pair of service provider VPCs and service consumer VPCs.

C. Create an internal Application Load Balancer in the service provider VPC and put application servers behind it.

D. Create a proxy server in the service provider VPC to route requests from service consumers to the application servers.

**Correct Answer with Provided Explanation:**

**A. Correct.** With a VPC endpoint, you can privately connect your VPC to supported AWS services and VPC endpoint services powered by AWS PrivateLink without requiring an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection. Instances in your VPC do not require public IP addresses to communicate with resources in the service. Traffic between your VPC and the other service does not leave the Amazon network.

**Explanation for why other options are wrong:**

* **Option B:** Establishing virtual private gateways for thousands of consumers would be an administrative nightmare and does not scale. It is also more complex to manage compared to the streamlined PrivateLink service.  
* **Option C:** While an internal ALB is part of the solution (it sits behind the Endpoint Service), an ALB on its own does not provide the cross-account, private connectivity requested.  
* **Option D:** A proxy server introduces a single point of failure and adds significant maintenance and scaling overhead that is avoided by using a managed service like PrivateLink.

## ---

**Question 11**

A solutions architect is designing a secure cloud-based application that uses Amazon EC2 instances within a VPC. The application uses other supported AWS services within the same Region. The network traffic between the instances and AWS services must remain private and must not travel across the public internet.

Which service or resource will meet the security requirement with the MOST operational efficiency?

A. Internet gateway.

B. NAT gateway.

C. VPC endpoint.

D. AWS Direct Connect.

**Correct Answer with Provided Explanation:**

**C. Correct.** VPC endpoints are virtual devices that make communication possible between services in your VPC and supported AWS services without the need to use the public internet. This solution provides a private path between your VPC and other AWS services.

**Explanation for why other options are wrong:**

* **Option A:** An Internet Gateway is used to route traffic to the public internet, which directly contradicts the requirement for traffic to remain private.  
* **Option B:** A NAT Gateway allows instances to reach the internet. While it provides some security by hiding private IPs, the traffic still traverses the public internet to reach the AWS service's public endpoints.  
* **Option D:** Direct Connect is used for private connectivity between on-premises and AWS. It is not designed for internal communication between VPC instances and regional AWS services.


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

**Question 18**

An application launched on Amazon EC2 instances needs to publish personally identifiable information (PII) about customers using Amazon Simple Notification Service (Amazon SNS). The application is launched in private subnets within an Amazon VPC.

What is the MOST secure way to allow the application to access service endpoints in the same AWS Region?

A. Use an internet gateway.

B. Use AWS PrivateLink.

C. Use a NAT gateway.

D. Use a proxy instance.

**Correct Answer with Provided Explanation:**

**B. Correct.** The use of PrivateLink does not require a public IP address on the instances or public access from the instance subnet. Traffic remains within the Region of the VPC and provides no single point of failure with the VPC endpoint. PrivateLink is the feature that powers VPC endpoints.

**Explanation for why other options are wrong:**

* **Option A:** Using an Internet Gateway requires the instances to have public IP addresses or be in a public subnet, which violates the security requirement to keep the instances in private subnets.  
* **Option C:** A NAT Gateway allows outbound traffic to the public internet. While it works, it is less secure than PrivateLink because the traffic leaves the VPC to reach the SNS public endpoint.  
* **Option D:** A proxy instance adds management overhead and creates a potential single point of failure and bottleneck compared to a managed AWS service like PrivateLink.

## ---

**Question 31**

A solutions architect is designing a hybrid application using the AWS Cloud. The network between the on-premises data center and AWS will use an AWS Direct Connect (DX) connection. The application connectivity between AWS and the on-premises data center must be highly resilient.

Which DX configuration should be implemented to meet these requirements?

A. Configure a DX connection with a VPN on top of it.

B. Configure a DX connection using the most reliable DX partner.

C. Configure multiple virtual interfaces on top of a DX connection.

D. Configure DX connections at multiple DX locations.

**Correct Answer with Provided Explanation:**

**D. Correct.** You can achieve maximum resiliency for critical workloads by using separate connections that terminate on separate devices in more than one location.

**Explanation for why other options are wrong:**

* **Option A:** While a VPN over DX provides encryption, it does not solve the physical resiliency requirement of the connection itself if the Direct Connect location fails.  
* **Option B:** Relying on a single partner or a single location is a "single point of failure." High resiliency requires physical redundancy across geographic locations.  
* **Option C:** Multiple virtual interfaces (VIFs) allow you to reach different VPCs or services, but they still run over the same physical fiber. If that fiber or the DX location goes down, all VIFs fail.

### ---

