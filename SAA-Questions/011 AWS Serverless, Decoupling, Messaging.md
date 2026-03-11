# ---

**TITLE 11: Serverless, Decoupling, and Messaging (SQS, SNS, Kinesis)**


## ---

**Question 21**

A web application runs on Amazon EC2 instances behind an Application Load Balancer (ALB). The application allows users to create custom reports of historical weather data. Generating a report can take up to 5 minutes. These long-running requests use many of the available incoming connections, making the system unresponsive to other users.

How can a solutions architect make the system more responsive?

A. Use Amazon SQS with AWS Lambda to generate reports.

B. Increase the idle timeout on the ALB to 5 minutes.

C. Update the client-side application code to increase its request timeout to 5 minutes.

D. Publish the reports to Amazon S3 and use Amazon CloudFront for downloading to the user.

**Correct Answer with Provided Explanation:**

**A. Correct.** If the company uses Amazon SQS and Lambda to offload the report generation, the system will no longer use resources on the web application servers to process the reports. Therefore, the system will no longer consume all the connections.

**Explanation for why other options are wrong:**

* **Option B:** Increasing the idle timeout allows the connection to stay open longer, but it does not solve the problem of connections being exhausted; in fact, it might make the problem worse.  
* **Option C:** Increasing the client timeout only prevents the client from giving up, but the server-side connection pool remains exhausted while the reports are being processed.  
* **Option D:** While S3 and CloudFront are good for delivering the final product, this option does not address the core issue of the web servers being overwhelmed during the generation process.

## ---

**Question 22**

An online photo application lets users upload photos and perform image editing operations. The application is built on Amazon EC2 instances and offers two classes of service: free and paid. Photos submitted by paid users are processed before those submitted by free users. Photos are uploaded to Amazon S3 and the job information is sent to Amazon SQS.

Which configuration should a solutions architect recommend?

A. Use one SQS FIFO queue. Assign a higher priority to the photos from paid users so they are processed first.

B. Use two SQS FIFO queues: one for paid users and one for free users. Set the free queue to use short polling and the paid queue to use long polling.

C. Use two SQS standard queues: one for paid users and one for free users. Configure the application on the Amazon EC2 instances to prioritize polling for the paid queue over the free queue.

D. Use one SQS standard queue. Set the visibility timeout of the photos from paid users to zero. Configure the application on the Amazon EC2 instances to prioritize visibility settings so photos from paid users are processed first.

**Correct Answer with Provided Explanation:**

**C. Correct.** Two separate queues are necessary for both free and paid requests. Prioritization would occur in the application that runs on the EC2 instances.

**Explanation for why other options are wrong:**

* **Option A:** Standard SQS queues (and FIFO) do not have a built-in "priority" attribute that allows one message to jump ahead of another in the same queue.  
* **Option B:** Polling type (Short vs. Long) affects cost and efficiency, not processing priority.  
* **Option D:** Setting visibility timeout to zero would make the message immediately available again, potentially leading to duplicate processing, and does not create a priority mechanism.

## ---

**Question 23**

A company built a food ordering application that captures user data and stores it for future analysis. The application's static front-end is deployed on an Amazon EC2 instance. The front-end application sends the requests to the backend application running on a separate EC2 instance. The backend application then stores the data in Amazon RDS.

What should a solutions architect do to decouple the architecture and make it scalable?

A. Use Amazon S3 to serve the static front-end application, which sends requests to Amazon EC2 to run the backend application. The backend application will process and store the data in Amazon RDS.

B. Use Amazon S3 to serve the static front-end application and write requests to an Amazon Simple Notification Service (Amazon SNS) topic. Subscribe the backend Amazon EC2 instance HTTP/S endpoint to the topic, and process and store the data in Amazon RDS.

C. Use an EC2 instance to serve the static front-end application and write requests to an Amazon SQS queue. Place the backend instance in an Auto Scaling group, and scale based on the queue depth to process and store the data in Amazon RDS.

D. Use Amazon S3 to serve the static front-end application and send requests to Amazon API Gateway, which writes the requests to an Amazon SQS queue. Place the backend instances in an Auto Scaling group, and scale based on the queue length to process and store the data in Amazon RDS.

**Correct Answer with Provided Explanation:**

**D. Correct.** A solution that moves the static frontend application to Amazon S3 will decouple the frontend application from the backend application. This solution will allow for scalability and improve application availability by removing the EC2 instance as a single point of failure. A serverless managed services like API Gateway and Amazon SQS would also eliminate single points of failure and further decouple the frontend requests from processing operations.

**Explanation for why other options are wrong:**

* **Option A:** This replaces only the front-end hosting; it does not "decouple" the communication between front-end and back-end, which remains a synchronous point-to-point call.

* **Option B:** SNS is a pub/sub service; while it decouples, it doesn't provide the "buffer" that SQS provides to handle spikes in food orders where the backend might need to catch up.  
* **Option C:** While it uses SQS, keeping the front-end on EC2 maintains a single point of failure and higher management overhead compared to using S3 for static hosting.

## ---

**Question 24**

A media company has an application that tracks user clicks on its websites and performs analytics to provide near-real-time recommendations. The application has a fleet of Amazon EC2 instances that receive data from the websites and send the data to an Amazon RDS DB instance for long-term retention. Another fleet of EC2 instances hosts the portion of the application that is continuously checking changes in the database and running SQL queries to provide recommendations.

Management has requested a redesign to decouple the infrastructure. The solution must ensure that data analysts are writing SQL to analyze the new data only. No data can be lost during the deployment.

What should a solutions architect recommend to meet these requirements and to provide the FASTEST access to the user activity?

A. Use Amazon Kinesis Data Streams to capture the data from the websites, Kinesis Data Firehose to persist the data on Amazon S3, and Amazon Athena to query the data.

B. Use Amazon Kinesis Data Streams to capture the data from the websites, Kinesis Data Analytics to query the data, and Kinesis Data Firehose to persist the data on Amazon S3.

**Correct Answer with Provided Explanation:**

**B. Correct.** This solution offers queries of the data before the data is sent to persistent storage. Kinesis Data Firehose retains the records for a default time of 24 hours if a replay is needed.

**Explanation for why other options are wrong:**

* **Option A:** Athena queries data after it is stored in S3. While effective, Kinesis Data Analytics (Option B) allows for querying the stream *in-flight*, providing even faster (real-time) recommendations.  
* **Option C:** SQS is a messaging service, not an analytics service. It doesn't allow for SQL-based querying of the data stream.  
* **Option D:** Lambda and SNS are not designed for high-volume SQL analytics on streaming clickstream data compared to the Kinesis suite.

## ---

**Question 25**

A solutions architect is redesigning a monolithic application to be a loosely coupled application composed of two microservices: Microservice A and Microservice B.

Microservice A places messages in a main Amazon Simple Queue Service (Amazon SQS) queue for Microservice B to consume. When Microservice B fails to process a message after four retries, the message needs to be removed from the queue and stored for further investigation.

What should the solutions architect do to meet these requirements?

A. Create an SQS dead-letter queue. Microservice B adds failed messages to that queue after it receives and fails to process the message four times.

B. Create an SQS dead-letter queue. Configure the main SQS queue to deliver messages to the dead-letter queue after the message has been received four times.

**Correct Answer with Provided Explanation:**

**B. Correct.** This solution uses a redrive policy on the main SQS queue to send messages to a dead-letter queue after consumption fails a certain number of times.

**Explanation for why other options are wrong:**

* **Option A:** This requires manual logic in the application code (Microservice B) to move the message. The redrive policy (Option B) is a native, automated feature of SQS.  
* **Option C:** Microservice A should only be responsible for sending messages, not tracking the failure status of Microservice B.  
* **Option D:** SQS queues do not "pull" from each other; the main queue "pushes" to the DLQ based on the redrive policy settings.

## ---

**Question 27**

A restaurant reservation application needs to access a waiting list. When a customer tries to reserve a table, and none are available, the customer application will put the user on the waiting list, and the application will notify the customer when a table becomes free. The waiting list must preserve the order in which customers were added to the waiting list.

Which service should the solutions architect recommend to store this waiting list?

A. Amazon Simple Notification Service (Amazon SNS)

B. AWS Step Functions invoking AWS Lambda functions.

C. A FIFO queue in Amazon Simple Queue Service (Amazon SQS)

D. A standard queue in Amazon Simple Queue Service (Amazon SQS)

**Correct Answer with Provided Explanation:**

**C. Correct.** Amazon SQS creates messages that exist in the queue until they are removed. FIFO queues respect the order in which items enter the queue. This solution would ensure that the person who has waited the longest for a table receives the next available table.

**Explanation for why other options are wrong:**

* **Option A:** SNS is a notification service and does not store messages or guarantee strict First-In-First-Out ordering for a waiting list.  
* **Option B:** Step Functions are for workflow orchestration; while powerful, they are over-engineered for a simple ordered waiting list compared to an SQS FIFO queue.  
* **Option D:** A Standard SQS queue provides "best-effort" ordering, but does not guarantee that messages will be delivered in the exact order they arrived.

## ---

**Question 28**

A company has a web application that makes requests to a backend API service. The API service runs on Amazon EC2 instances accessed behind an Elastic Load Balancer.

Most backend API service endpoint calls finish very quickly, but one endpoint that makes calls to create objects in an external service takes a long time to complete. These long-running calls are causing client timeouts and increasing overall system latency.

What should be done to minimize the system throughput impact of the slow-running endpoint?

A. Change the EC2 instance size to increase memory and compute capacity.

B. Use Amazon Simple Queue Service (Amazon SQS) to offload the long-running requests for asynchronous processing by separate workers.

C. Increase the load balancer idle timeout to allow the long-running requests to complete.

D. Use Amazon ElastiCache for Redis to cache responses from the external service.

**Correct Answer with Provided Explanation:**

**B. Correct.** You can move the long-running calls off the interactive request path so that separate resources can complete them asynchronously. This solution ensures that your backend API service does not exhaust its incoming connection resources.

**Explanation for why other options are wrong:**

* **Option A:** Increasing instance size might help processing speed, but it doesn't solve the architectural problem of a synchronous bottleneck waiting on an external service.  
* **Option C:** Increasing timeout just keeps connections occupied longer, which actually decreases overall system throughput and availability for other users.  
* **Option D:** Caching helps with data retrieval, but the question is about an endpoint that "creates objects," which is an action that typically cannot be served from a cache.

### ---

