Here is the training sheet for **Title 2: Account Security**, organized according to your specific formatting requirements.

---

# TITLE 2: Account Security (IAM, Organizations, Policies, Roles)

---

## Question 2

A company has implemented one of its microservices on AWS Lambda that accesses an Amazon DynamoDB table named "Books". A solutions architect is designing an IAM policy to be attached to the Lambda function's IAM role giving it access to put, update, and delete items in the "Books" table. The IAM policy must prevent the function from performing any other actions on the "Books" table and any other table.
Which IAM policy would fulfill these needs and provide the LEAST privileged access?

**A.**

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PutUpdateDeleteOnBooks",
            "Effect": "Allow",
            "Action": [
                "dynamodb:PutItem",
                "dynamodb:UpdateItem",
                "dynamodb:DeleteItem"
            ],
            "Resource": "arn:aws:dynamodb:us-west-2:123456789012:table/Books"
        }
    ]
}

```

**B.** 
```json
{
"Version": "2012-10-17",
"Statement": [
{
"Sid": "PutUpdateDeleteOnBooks",
"Effect": "Allow",
"Action": [
"dynamodb:PutItem",
"dynamodb:UpdateItem",
"dynamodb:DeleteItem"
],
"Resource": "arn:aws:dynamodb:us-west-2:123456789012:table/*"
}
]
}

```

**C.** 
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PutUpdateDeleteOnBooks",
            "Effect": "Allow",
            "Action": "dynamodb:*",
            "Resource": "arn:aws:dynamodb:us-west-2:123456789012:table/Books"
        }
    ]
}

```

**D.**
 ```json
{
"Version": "2012-10-17",
"Statement": [
{
"Sid": "PutUpdateDeleteOnBooks",
"Effect": "Allow",
"Action": "dynamodb:*",
"Resource": "arn:aws:dynamodb:us-west-2:123456789012:table/Books"
},
{
"Sid": "PutUpdateDeleteOnBooks",
"Effect": "Deny",
"Action": "dynamodb:*:*",
"Resource": "arn:aws:dynamodb:us-west-2:123456789012:table/Books"
}
]
}

```



**Correct Answer with Provided Explanation:**
**A. Correct.** This policy will provide the required Put, Update, and Delete actions to only the "Books" table.

**Explanation for why other options are wrong:**
* **Option B:** Uses a wildcard (`*`) in the Resource element. This would allow the Lambda function to access every table in the account, violating the principle of least privilege.
* **Option C:** Uses a wildcard (`*`) in the Action element. This grants full control over the "Books" table (including deleting the table itself), which goes beyond the required Put, Update, and Delete permissions.
* **Option D:** The policy is redundant and contains a broad `Deny` statement (`dynamodb:*:*`) that could potentially conflict with the `Allow` statement or lead to unintended behavior; it also still incorrectly uses `dynamodb:*` for the allowed actions.

---

## Question 3
An administrator wants to apply a resource-based policy to the S3 bucket named "iam-policy-testbucket" to restrict access and to allow accounts to only write objects to the bucket. When the administrator tries to apply the following policy to the "iam-policy-testbucket" bucket, the S3 bucket presents an error.
`{ "Version": "2012-10-17", "Id": "Policy1646946718956", "Statement": [ { "Sid": "Stmt1646946717210", "Effect": "Allow", "Action": "s3:PutObject", "Resource": "arn:aws:s3:::iam-policy-testbucket/*" } ]}`
How can the administrator correct the policy to resolve the error and successfully apply the policy?

A. Change the Action element from s3:PutObject to s3:*.
B. Remove the Resource element because it is unnecessary for resource-based policies.
C. Change the Resource element to NotResource.
D. Add a Principal element to the policy to declare which accounts have access.

**Correct Answer with Provided Explanation:**
**D is Correct.** Resource-based policies must include a Principal element in the policy.

**Explanation for why other options are wrong:**
* **Option A:** Changing the action to a wildcard doesn't address the missing structural component (Principal) required by all S3 bucket policies.
* **Option B:** The Resource element is mandatory in S3 bucket policies to specify the objects or bucket the policy governs.
* **Option C:** Using `NotResource` would allow the action on everything *except* the bucket, which is the opposite of the administrator's intent and still wouldn't fix the missing Principal error.

---

## Question 4
A company designs a mobile app for its customers to upload photos to a website. The app needs a secure login with multi-factor authentication (MFA). The company wants to limit the initial build time and the maintenance of the solution.
Which solution should a solutions architect recommend to meet these requirements?

A. Use Amazon Cognito Identity with SMS-based MFA.
B. Edit IAM policies to require MFA for all users.
C. Federate IAM against the corporate Active Directory that requires MFA
D. Use Amazon API Gateway and require server-side encryption (SSE) for photos.



**Correct Answer with Provided Explanation:**
**A. Correct.** Amazon Cognito user pools are user directories that provide sign-up and sign-in options for web users and mobile app users. You can add MFA to Amazon Cognito user pools for secondary validation.

**Explanation for why other options are wrong:**
* **Option B:** IAM is designed for AWS resource management by employees, not for managing thousands of external mobile app customers.
* **Option C:** While possible, setting up federation with a corporate AD is complex and increases maintenance overhead compared to the managed Cognito service.
* **Option D:** API Gateway and SSE provide security for data in transit and at rest, but they do not provide a user identity management or MFA solution.

---

## Question 5
A solutions architect is designing a new workload in which an AWS Lambda function will access an Amazon DynamoDB table.
What is the MOST secure means of granting the Lambda function access to the DynamoDB table?

A. Create an IAM role with the necessary permissions to access the DynamoDB table. Assign the role to the Lambda function.
B. Create a DynamoDB user name and password and give them to the developer to use in the Lambda function.
C. Create an IAM user, and create access and secret keys for the user. Give the user the necessary permissions to access the DynamoDB table. Have the developer use these keys to access the resources.
D. Create an IAM role allowing access from AWS Lambda. Assign the role to the DynamoDB table.

**Correct Answer with Provided Explanation:**
**A. Correct.** An IAM role is an IAM entity that defines a set of permissions for making AWS service requests. IAM roles are not associated with a specific user or group. Instead, trusted entities such as IAM users, applications, or AWS services assume roles.

**Explanation for why other options are wrong:**
* **Option B:** DynamoDB is a web service that uses IAM for authentication; it does not support legacy "username and password" credentials for data access.
* **Option C:** Creating an IAM user requires managing long-term credentials (Access Keys). This is less secure than using IAM roles, which provide temporary credentials.
* **Option D:** This is conceptually backward. IAM roles are assigned to the "principal" doing the work (the Lambda function), not the "resource" being accessed (the DynamoDB table).

---

## Question 6
A company has an application running as a service in Amazon Elastic Container Service (Amazon ECS) using the Amazon EC2 launch type. The application code makes AWS API calls to publish messages to Amazon Simple Queue Service (Amazon SQS).
What is the MOST secure method of giving the application permission to publish messages to Amazon SQS?

A. Use AWS Identity and Access Management (IAM) to grant SQS permissions to the role used by the launch configuration for the Auto Scaling group of the ECS cluster.
B. Create a new IAM user with SQS permissions. Then update the task definition to declare the access key ID and secret access key as environment variables.
C. Create a new IAM role with SQS permissions. Then update the task definition to use this role for the task role setting.
D. Update the security group used by the ECS cluster to allow access to Amazon SQS.

**Correct Answer with Provided Explanation:**
**C. Correct.** The creation of a new IAM role (based on the minimal Amazon Elastic Container Service Task Role template) ensures that only the containers that are created with this task will receive access to Amazon SQS.

**Explanation for why other options are wrong:**
* **Option A:** Assigning permissions to the EC2 instance role (launch configuration) grants access to *every* container running on that host, violating the principle of isolation and least privilege.
* **Option B:** Storing Access Keys in environment variables is a security risk as they are long-term credentials and can be exposed via the ECS console or API.
* **Option D:** Security groups are network-level firewalls (IPs and Ports); they cannot grant API-level permissions to AWS services like SQS.

---

## Question 9
A company is performing an AWS Well-Architected Framework review of an existing workload deployed on AWS. The review identified a public-facing website running on the same Amazon EC2 instance as a Microsoft Active Directory domain controller that was installed recently to support other AWS services. A solutions architect needs to recommend a new design that would improve the security of the architecture and minimize the administrative demand on IT staff.
What should the solutions architect recommend?

A. Use AWS Managed Microsoft AD to create a managed Active Directory. Uninstall Active Directory on the current EC2 instance.
B. Create another EC2 instance in the same subnet and reinstall Active Directory on it. Uninstall Active Directory on the current EC2 instance.
C. Use AWS Directory Service to create an Active Directory connector. Proxy Active Directory requests to the Active Directory domain controller running on the current EC2 instance.
D. Configure AWS IAM Identity Center (AWS Single Sign-On) with Security Assertion Markup Language (SAML) 2.0 federation with the current Active Directory controller. Modify the EC2 instance's security group to deny public access to Active Directory.



**Correct Answer with Provided Explanation:**
**A. Correct.** AWS Managed Microsoft AD is powered by an actual Microsoft Windows Server Active Directory that AWS manages in the AWS Cloud. This configuration reduces administrative overhead and is secure. Additionally, you can mitigate security risks by uninstalling Active Directory from the current server so the Active Directory and the website are not on the same server.

**Explanation for why other options are wrong:**
* **Option B:** While this separates the services, it does not minimize administrative demand because the IT staff still must manage, patch, and backup the new EC2 instance themselves.
* **Option C:** AD Connector is just a gateway; it still requires the original, unmanaged domain controller to be running, which doesn't solve the security or management issues.
* **Option D:** This addresses authentication for AWS access but doesn't fix the architectural risk of running a domain controller on a public-facing web server.

---
