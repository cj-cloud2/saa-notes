# ---

**TITLE 3: Networking Part 1 (VPC, Subnets, Security Groups, NACLs, IGW, Route Tables)**

## ---

**Question 7**

A solutions architect needs to allow developers to have SSH connectivity to web servers. The requirements are as follows:

• Limit access to users originating from the corporate network.

• Web servers cannot have SSH access directly from the internet.

• Web servers reside in a private subnet.

Which combination of steps must the architect complete to meet these requirements? (Select TWO.)

A. Create a bastion host that authenticates users against the corporate directory.

B. Create a bastion host with security group rules that only allow traffic from the corporate network.

C. Attach an IAM role to the bastion host with relevant permissions.

D. Configure the web servers' security group to allow SSH traffic from a bastion host.

E. Deny all SSH traffic from the corporate network in the inbound network ACL.

**Correct Answer with Provided Explanation:**

**B and D are Correct.**

**B. Correct.** A bastion host meets the requirement to prevent direct SSH access from the internet. The addition of a security group rule that only allows SSH access from the corporate network meets the requirement to limit access accordingly.

**D. Correct.** The configuration of the web servers' security group to only allow SSH access from the bastion host will prevent access from other systems.

**Explanation for why other options are wrong:**

* **Option A:** While directory authentication is a good security layer, it does not fulfill the networking requirement to keep the web servers in a private subnet and inaccessible from the direct internet.  
* **Option C:** IAM roles provide permissions for the instance to call AWS APIs, but they do not control SSH network-level access or routing.  
* **Option E:** Denying traffic from the corporate network would prevent the developers from ever reaching the bastion host, making connectivity impossible.

## ---

**Question 8**

A company hosts a popular web application. The web application connects to a database running in a private VPC subnet. The web servers must be accessible only to customers on an SSL connection. The Amazon RDS for MySQL database server must be accessible only from the web servers.

How should a solutions architect design a solution to meet the requirements without impacting running applications?

A. Create a network ACL on the web server's subnet, and allow HTTPS inbound and MySQL outbound. Place both database and web servers on the same subnet.

B. Open an HTTPS port on the security group for web servers and set the source to 0.0.0.0/0. Open the MySQL port on the database security group and attach it to the MySQL instance. Set the source to web server security group.

C. Create a network ACL on the web server's subnet; allow HTTPS inbound, and specify the source as 0.0.0.0/0. Create a network ACL on a database subnet, allow MySQL port inbound for web servers, and deny all outbound traffic.

D. Open the MySQL port on the security group for web servers and set the source to 0.0.0.0/0. Open the HTTPS port on the database security group and attach it to the MySQL instance. Set the source to web server security group.

**Correct Answer with Provided Explanation:**

**B. Correct.** If you leave the database in a private subnet and leave the web server in a public subnet, you restrict internet access to only the application server. One security group for each tier enforces least-privilege access. Security groups are stateful. Therefore, if a path is opened, then the return path is also open.

**Explanation for why other options are wrong:**

* **Option A:** Placing both tiers in the same subnet is a poor security practice. Furthermore, NACLs are stateless, so only allowing "outbound" or "inbound" without the corresponding return path would break connectivity.  
* **Option C:** Denying all outbound traffic on a NACL would prevent the database from sending response data back to the web server, as NACLs require both inbound and outbound rules to be explicitly set for a conversation to work.  
* **Option D:** This logic is reversed; the web server should handle HTTPS traffic from the internet, and the database should handle MySQL traffic from the web server.

## ---

**Question 10**

A company is reviewing a recent migration of a three-tier application to a VPC. The security team discovers that the principle of least privilege is not being applied to Amazon EC2 security group ingress and egress rules between the application tiers.

What should a solutions architect do to correct this issue?

A. Create security group rules using the instance ID as the source or destination.

B. Create security group rules using the security group ID as the source or destination.

C. Create security group rules using the VPC CIDR blocks as the source or destination.

D. Create security group rules using the subnet CIDR blocks as the source or destination.

**Correct Answer with Provided Explanation:**

**B. Correct.** Security group rules are used to control inbound and outbound traffic that can reach instances associated with a security group. You can specify the security group ID as the source or destination. This solution ensures that the security group rules are implemented correctly and that application tiers are able to communicate.

**Explanation for why other options are wrong:**

* **Option A:** AWS Security Groups do not support using an individual Instance ID as a source or destination in a rule.

* **Option C:** Using the VPC CIDR is too broad; it allows any resource within the entire VPC to communicate with the tier, failing the "least privilege" requirement.  
* **Option D:** Using Subnet CIDRs is also too broad. If you add different types of instances to the same subnet later, they would automatically gain access they might not need. Referencing Security Group IDs ensures only specific application components have access.

## ---

**Question 11**

A solutions architect is designing a secure cloud-based application that uses Amazon EC2 instances within a VPC. The application uses other supported AWS services within the same Region. The network traffic between the instances and AWS services must remain private and must not travel across the public internet.

Which service or resource will meet the security requirement with the MOST operational efficiency?

A. Internet gateway.

B. NAT gateway.

C. VPC endpoint

D. AWS Direct Connect

**Correct Answer with Provided Explanation:**

**C. Correct.** VPC endpoints are virtual devices that make communication possible between services in your VPC and supported AWS services without the need to use the public internet. This solution provides a private path between your VPC and other AWS services.

**Explanation for why other options are wrong:**

* **Option A:** An Internet Gateway (IGW) explicitly routes traffic over the public internet, which violates the primary security requirement of the question.  
* **Option B:** A NAT Gateway allows instances in a private subnet to connect to the internet. While it hides the instance IP, the traffic still traverses the public internet to reach AWS service endpoints.  
* **Option D:** Direct Connect is a physical connection between an on-premises data center and AWS. While private, it is not used for internal VPC-to-AWS service communication and is far less operationally efficient for this specific use case.

### ---

