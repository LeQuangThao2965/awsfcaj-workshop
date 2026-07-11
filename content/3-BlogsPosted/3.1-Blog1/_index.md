---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

## Monitoring and Managing Claude Code Costs on Amazon Bedrock: A Comprehensive Guide for Developers
Using AI in programming is an irreversible trend, and Claude Code combined with Amazon Bedrock is one of the most powerful tools available today. However, when deploying this tool for an entire team, managers and system engineers often face a tough question: How do we track usage, control costs, and measure actual code generation performance?

Fortunately, AWS provides a built-in monitoring solution through OpenTelemetry (OTEL) and CloudWatch. Let's explore the architecture behind this system to see how you can leverage it.

## 1. Two Monitoring Architecture Options: Sidecar vs. Central

Depending on your project team size and budget, you can choose to deploy the data collection system (Collector) in one of the following two models:

### Sidecar Model (Economical & Lightweight)

If you want a monitoring solution with no server maintenance costs (around $0/month for infrastructure), Sidecar is the perfect choice.

- How it works: A very lightweight Go application (named otel-helper, only about 15-20MB) runs in the background directly on each developer's machine.
- Advantages: Each client sends metrics directly to the CloudWatch OTLP endpoint via SigV4 authentication. The system requires absolutely no setup for VPC, Load Balancer, or ECS Fargate.

### Central Model (Centralized & For Enterprise)

If your company needs long-term historical data storage for analytical SQL queries, the Central model is mandatory.

- How it works: Data from client machines is sent to a central server (running ECS Fargate) placed behind an Application Load Balancer (ALB).
- Cost: Slightly higher, around $30–$50/month for AWS resources.
- Power: Enables deep data analysis workflows with Athena (covered in the next section).

![alt text](../../images/3-BlogsPosted/3.1-Blog1/image1.png)

## 2. What Is The System Listening To?

More than just dry numbers, this system sends back highly practical data reflecting developer behavior and performance:

- Consumption & Costs: Closely monitors Token volume (input/output/cache) and estimates AWS costs in real-time (metrics token.usage, cost.usage).
- Programming Performance: Records the number of lines of code added/deleted (lines_of_code.count), the number of commits, and Pull Requests (pull_request.count).
- AI Usage Behavior: What code editing decisions is the AI tool making? How long is the developer actually "active" working with the AI?

All these metrics are tagged in detail by user (user.email), the type of AI model being used, and even the department/team if you use OIDC authentication.

![alt text](../../images/3-BlogsPosted/3.1-Blog1/image2.png)

## 3. Visual Dashboard With PromQL

All collected data will be pushed to CloudWatch Dashboards. Thanks to the power of the PromQL query language, you will instantly get a sleek dashboard with charts:

- Overview of active users and Cache hit rate (helping to save API costs).
- Cost and token distribution charts by user or team.
- Code generation performance analysis by programming language.

![alt text](../../images/3-BlogsPosted/3.1-Blog1/image3.png)

## 4. Budget Management (Quota) - Preventing Accidental "Money Burning"

Giving developers a powerful AI tool without budget limits is a major cloud cost risk. This monitoring solution integrates a highly intelligent budget control mechanism:

- Every 15 minutes, a background AWS Lambda function runs, queries token metrics from CloudWatch, and updates them into a DynamoDB table.
- When a developer uses Claude Code, the system quickly checks against the quota limit on DynamoDB to make a swift "Allow" or "Block" decision.

## 5. Unlocking Historical Analysis Power With AWS Athena (Optional)

PromQL CloudWatch Dashboards are great for real-time monitoring, but they are limited to a 7-day data retention period.

If you are a senior manager who wants to create quarterly reports, you can enable the analytics_enabled=true feature (only available on the Central model). At this point, the system will send additional logs in EMF (Embedded Metric Format). This data is aggregated by Kinesis Data Firehose, converted into highly optimized Parquet format, and stored in S3.

From here, you can easily use AWS Athena to run SQL queries analyzing the work history of hundreds of developers over several months.

## Conclusion

Whether you are a small team wanting to use the free Sidecar model for quick metrics, or an enterprise needing the Central model with Kinesis & Athena to manage thousands of developers, this AWS monitoring architecture can meet your needs. By setting up this telemetry system, you not only control infrastructure costs but also measure exactly how much value AI is bringing to your project.

### Original reference links:

- [AWS Solutions Library: Monitoring Claude Code with Amazon Bedrock](https://github.com/aws-solutions-library-samples/guidance-for-claude-code-with-amazon-bedrock/blob/main/assets/docs/MONITORING.md)
- [AWS Bedrock Generative AI Application Architecture | AWS Builder Center](https://builder.aws.com/content/2f2d59922DQNz3iH1pCTeudpmhv/aws-bedrock-generative-ai-application-architecture)
- [Managing AWS Distro for OpenTelemetry Collector | AWS Open Source Blog](https://aws.amazon.com/vi/blogs/opensource/managing-aws-distro-for-opentelemetry-collector-with-aws-systems-manager-distributor/)
- [Getting started with CloudWatch automatic dashboards - Amazon CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/GettingStarted.html)