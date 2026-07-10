---
title: "Blog 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

## AWS Weekly Quick Summary: Claude Opus 4.8, Aurora MySQL + Kiro, AWS Transform, and Many Notable Updates

This week, AWS brought quite a few new updates, from AI and databases to the ability to assess infrastructure before migrating to the cloud. Below are some of the points I found noteworthy.

## Claude Opus 4.8 is now available on AWS

Anthropic's most powerful model currently, **Claude Opus 4.8**, is now available through **Amazon Bedrock** and the Claude platform on AWS.

The highlight of this version is its ability to handle long tasks, automated programming, and better inference. For developers, Claude can read the entire codebase, plan ahead before making changes, and maintain context throughout a long work session instead of just processing individual code segments.

If using Amazon Bedrock, you still benefit from AWS managed services such as Guardrails, Knowledge Bases, and integrated data management capabilities.

## Next-Generation AWS Resilience Hub

AWS has also upgraded its **Resilience Hub** to help SREs and DevOps better manage system resilience.

What I find particularly useful is that this service not only assesses application readiness but also supports building SLO policies, Disaster Recovery, dependency detection, and AI-powered assessments based on the AWS Well-Architected Framework.

If you're running a lot of workloads, this will be a very useful tool for monitoring fault tolerance across your organization.

## AWS Transform Adds Pre-Migration Assessment Capabilities

In addition to the previously introduced Continuous Modernization feature, AWS Transform now supports pre-migration assessments for systems to AWS.

The service can import data from RVTools, CMDB, or other discovery tools to estimate TCO, analyze application modernization feasibility, and assess readiness in minutes per repository.

In my opinion, this gives businesses a clearer picture of costs and complexity before starting a migration project.

## Aurora MySQL with Kiro Powers

A rather interesting update is that **Aurora MySQL** has integrated with **Kiro Powers**.

Developers can now use natural language to perform many tasks such as:

* Querying data.

* Schema management.

* Scaling Aurora Serverless.

* Migrating from RDS to Aurora.

* Setting up replication.

Instead of manual manipulation or writing many SQL statements, AI will generate API calls, SQL, and configurations for developers to review before execution.

## OpenSearch Serverless for Agent AI

AWS also introduced a new generation of **OpenSearch Serverless**, optimized for AI agent applications.

The service supports vector search, scaling from zero to thousands of requests per second, and integrates directly with tools like Cursor, Claude Code, Kiro, and Vercel through Agent Skills.

## Conclusion

The direction AWS has been quite clear recently: integrating AI into the entire software development lifecycle, from coding and infrastructure assessment to database management, operation, and application modernization.

In my opinion, instead of just focusing on source code generation, AWS is building an ecosystem where AI becomes a "teammate" for developers at every stage of software development.

Source: https://aws.amazon.com/blogs/aws/