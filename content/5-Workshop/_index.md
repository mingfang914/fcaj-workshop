---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---
# Smart Image Platform Setup Lab

#### Overview

The **Smart Image Platform** is an AWS Serverless & Event-Driven application designed to automate image uploading, processing (resizing, thumbnail generation), and AI-based image classification and tagging.

In this workshop, this section covers how to build and configure the entire infrastructure of this platform **step-by-step using the AWS Management Console**. This will help understand how each service is connected, from user authentication to S3 object notifications, Lambda function execution, and AI integration.

> [!NOTE]
> In the official project (**Smart Image Platform**), the cloud infrastructure is fully automated and deployed using the **AWS Cloud Development Kit (CDK)** as Infrastructure as Code (IaC). However, in this hands-on practice section (Workshop), configurations are performed manually on the AWS Management Console UI to ensure visual clarity.

#### Content

1. [Workshop Overview](5.1-Workshop-overview/)
2. [Prerequisites](5.2-Prerequisites/)
3. [Storage & Database Configuration](5.3-Storage-Database/)
4. [User Authentication with Amazon Cognito](5.4-Cognito-Auth/)
5. [Serverless Backend & Triggers](5.5-Backend-Serverless/)
6. [Frontend Deployment with AWS Amplify](5.6-Frontend-Amplify/)
7. [Testing & Verification](5.7-Testing-Validation/)
8. [Resource Cleanup](5.8-Cleanup/)