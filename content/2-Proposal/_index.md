---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Cloud-based Digital Product Marketplace with 3D Preview
## A marketplace solution for digital products with integrated 3D preview, real-time payment, and AWS-based storage

### 1. Executive Summary
*About the project* <br>
The "Cloud-based Digital Product Marketplace with 3D Preview on AWS" project is a marketplace platform for digital products such as PDF/Word documents, 3D models, design assets, and other digital resources.


The system allows users to register accounts, log in, search for products, view detailed information, pay online, and access products once a transaction is successfully confirmed.
A highlight of the project is the ability to display 3D models directly on the web interface for 3D Model products. Users can rotate, zoom in, zoom out, and inspect a product before buying, increasing visual clarity compared to ordinary digital-document websites.


Technically, the system uses React on the Frontend, Node.js with Express on the Backend, and a database to store information about users, products, orders, transactions, and access rights. The deployment infrastructure is expected to use Amazon EC2 to run the Backend application, Amazon S3 to store product files, AWS IAM to control access to AWS resources, and SePay to test the real-time payment process.

*Objectives*
<br>
The goal of the project is to apply the AWS knowledge learned in the workshop to a real-world problem, including deploying a web application to the cloud, managing files with object storage, designing resource access permissions, optimizing operating costs, and building a system capable of future expansion.


### 2. Problem Statement
*Current problem*
Many platforms for sharing or trading digital products focus only on uploading files and handling basic transactions. For highly visual products such as 3D models, game assets, or design resources, buyers often find it hard to assess quality if they can only see illustrative images or text descriptions.

If all product files are stored directly on the application server, the system faces many limitations such as difficulty scaling storage, dependence on the server's hard drive, difficulty managing files as the number of products grows, and potential risk of data loss when the server fails.

In addition, a manual payment confirmation process is inconvenient for both buyers and sellers. If the administrator has to check bank transfers by hand, order confirmation will be slow, error-prone, and unsuitable for an automated marketplace model.

*Solution*
The project proposes building a marketplace for digital products, in which the web application is responsible for managing users, products, transactions, and post-purchase access rights. Product files are stored separately on Amazon S3 instead of directly on the server. The Node.js + Express Backend acts as the intermediary handling business logic, authenticating users, managing products, checking transactions, and communicating with AWS services.

For 3D Model products, the React Frontend integrates a 3D Viewer feature to display the model directly on the web. For PDF/Word documents, the system will continue to refine the content preview feature before purchase, such as limiting the number of preview pages or showing a sample preview.

The payment flow is integrated with SePay to automatically receive transaction notifications in real time. When a user transfers money successfully, the Backend processes the webhook/API data from SePay, reconciles the order code, updates the transaction status, and grants product access to the buyer.

**Benefits and value delivered:**
<br>&emsp;- Simulates a real marketplace system with buyer, seller, and administrator flows.
<br>&emsp;- Separates compute and storage by deploying the application on EC2 and storing files on S3.
<br>&emsp;- Enhances the user experience through 3D model preview on the web.
<br>&emsp;- Automates payment confirmation through SePay, reducing manual checking.
<br>&emsp;- Applies IAM and the least-privilege principle to control access to AWS resources.


### 3. Solution Architecture
The system is designed as a web application deployed on AWS, comprising the main components: Client, Frontend, Backend, Database, Amazon S3, SePay, and IAM. Users access the React interface; the Frontend sends requests to the Node.js + Express Backend via REST API; the Backend handles business logic, connects to the database, communicates with S3, and processes payments through SePay.
```
Temporary flow (diagram not yet drawn)
User Browser
    -> React Frontend
        -> Node.js + Express Backend on Amazon EC2
            -> Database
            -> Amazon S3 (PDF, Word, images, 3D models)
            -> SePay API / Webhook
            -> AWS IAM Role / Policy
```
{{% notice note %}}
**Note:** The official architecture diagram will be added in the final report after the trial deployment configuration on AWS is completed.
{{% /notice %}}

![IoT Weather Station Architecture](../images/2-Proposal/edge_architecture.jpeg)

![IoT Weather Platform Architecture](../images/2-Proposal/platform_architecture.jpeg)

***AWS services used***
- **React**: Builds the user interface, product pages, detail pages, the admin area, and the 3D preview area.
- **Node.js + Express**: Builds the Backend API, handles marketplace business logic, authentication, product management, transactions, and external service integration.
- **Database**: Stores user, product, category, order, transaction, and product-access data.
- **Amazon EC2**: The deployment environment for the Backend, running Node.js + Express and handling requests from the Frontend.
- **Amazon S3**: Stores digital product files such as PDF, Word, images, thumbnails, and 3D models.
- **AWS IAM**: Manages S3 access and limits permissions according to the least-privilege principle.
- **AWS Budget**: Tracks and alerts on costs during testing.
- **SePay**: Integrates payment and handles transaction notifications in real time.
- **GitHub / GitHub Actions**: Manages source code and can be extended to CI/CD in a later phase.


***Component design***
- **React Frontend**: displays the user interface, product list, product details, user administration, and the 3D viewer.
- **Node.js + Express Backend**: handles the REST API, validates data, checks access rights, operates on the database, and calls SePay and S3.
- **Database**: stores the users, products, categories, orders, transactions, product_files, and user_purchases tables.
- **Amazon S3**: stores objects in a logical structure such as products/{productId}/thumbnail.jpg, products/{productId}/document.pdf, and products/{productId}/model.glb.
- **Admin**: manages users, bans/unbans accounts, reviews products, and monitors system status.
- **Seller**: posts products, updates products, and uploads product files to the system.
- *Buyer*: searches, previews, pays, and accesses products after purchase.


### 4. Technical Implementation
*Implementation phases*
Phase   |   Work performed
|---|---|
|Phase 1: Research and topic selection | Study EC2, S3, IAM, AWS Budget, and the web application deployment process; select the digital-product marketplace topic with integrated 3D Preview.|
|Phase 2: Architecture design and project initialization | Initialize React + Node.js + Express; build the frontend/backend structure; define the Authentication, Product Management, User Management, Payment, and File Storage modules.|
|Phase 3: Core feature development | Develop registration, login, product search, product display, user management, ban/unban, and 3D model display.|
|Phase 4: Payment and file storage integration | Integrate SePay for real-time payment; prepare the upload flow and product file management on Amazon S3.|
|Phase 5: AWS deployment and testing | Deploy the Backend to EC2, configure the S3 bucket, IAM Policy/Role, test the main flows, and finalize the report.|


*Technical requirements*
- The Backend supports a REST API, authentication via session/JWT, data validation, file upload handling, database connectivity, and communication with SePay/S3.
- The Frontend supports a responsive interface, product list, login/registration forms, product detail page, admin page, and a 3D Viewer.
- AWS requires an EC2 instance for the test environment, a Security Group opening the necessary ports, an S3 bucket to store product files, an IAM policy limiting access, and AWS Budget to track costs.
- The system must check access rights after purchase to ensure users can only download/view in full the products they have successfully paid for.

*Current prototype status*
- Registration, login, and product search are implemented.
- A 3D View Model interface is available for 3D Model products.
- Basic user management is implemented, including ban/unban user.
- Real-time payment via SePay is integrated, along with money-transfer notification handling to confirm successful transactions.
- AWS deployment, the official architecture diagram, seller registration, category management, and the pre-purchase document preview feature are not yet complete.



### 5. Timeline & Milestones
Milestone | Main goal
|---|---|
|Month 1 | Learn the AWS fundamentals, practice EC2/S3/IAM/AWS Budget, test deploying a sample application, and select the topic.|
|Month 2 | Design the EC2 + S3 + IAM architecture, initialize the React + Node.js + Express project, build the database, and develop the MVP.|
|Month 3 | Integrate SePay payment, finalize the buyer/seller/admin flows, trial-deploy to EC2, integrate S3, and finalize the report.|

- *Post-deployment*: Continue researching and developing the product for one more year.

### 6. Budget Estimation

The project is designed to be cost-efficient in a learning and testing environment. The main services are Amazon EC2, Amazon S3, AWS IAM, and AWS Budget. Actual costs will be updated with the AWS Pricing Calculator after finalizing the Region, instance type, S3 capacity, and EC2 runtime.
```
You can view the costs on the [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=621f38b12a1ef026842ba2ddfe46ff936ed4ab01)
Or download the [budget estimation file](../attachments/budget_estimation.pdf).
```
```
*Infrastructure costs*
- AWS Lambda: $0.00/month (1,000 requests, 512 MB storage).
- S3 Standard: $0.15/month (6 GB, 2,100 requests, 1 GB scanned).
- Data transfer: $0.02/month (1 GB in, 1 GB out).
- AWS Amplify: $0.35/month (256 MB, 500 ms requests).
- Amazon API Gateway: $0.01/month (2,000 requests).
- AWS Glue ETL Jobs: $0.02/month (2 DPUs).
- AWS Glue Crawlers: $0.07/month (1 crawler).
- MQTT (IoT Core): $0.08/month (5 devices, 45,000 messages).

*Total*: $0.7/month, $8.40/12 months
- *Hardware*: $265 one-time (Raspberry Pi 5 and sensors).
```

### 7. Risk Assessment
Risk|	Impact|	Probability|	Mitigation strategy
|---|---|---|---|
|Not yet successfully deployed to AWS	|High	|Medium	|Prioritize deploying the Backend to EC2 first, then integrate S3 and the secondary parts.|
|S3 integration encounters access permission errors	|Medium	|Medium	|Design a clear IAM policy and test each permission: PutObject, GetObject, DeleteObject.|
|PDF/Word preview not yet complete	|Medium	|High	|Prioritize PDF preview first; for Word, a PDF preview or thumbnail can be used as a substitute.|
|Payment transaction status mismatch	|High	|Medium	|Check the order code, amount, transaction status, and handle webhook deduplication.|
|AWS cost overrun during testing	|Medium	|Low	|Enable AWS Budget, limit S3 capacity, and shut down EC2 when not in use.|


### 8. Expected Outcomes
- Build a digital-product marketplace with basic features such as registration, login, search, product detail viewing, user management, and product management.
- Support displaying 3D models directly on the web for 3D Model products.
- Integrate a real-time payment process through SePay and automatically update the transaction status.
- Trial-deploy the Backend to Amazon EC2 and use Amazon S3 to store digital product files.
- Apply AWS IAM to control access to cloud resources following basic security principles.
- Form a clear system architecture suitable for presentation in the workshop report and capable of future expansion.
In terms of learning value, the project helps students better understand the process of deploying a real web application to AWS, how to separate compute and storage, how to design a backend API, how to handle online payments, and how to control resource access following basic security principles.

In terms of scalability, the system can add features such as seller registration, product review before listing, advanced category management, product ratings, seller wallets, revenue sharing, pre-purchase document preview, and automated CI/CD with GitHub Actions.
