---
title: "Prerequisites"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

# Prerequisites

## 1. AWS account and permissions

- Do not use the root user for daily operations.
- Prefer IAM Identity Center or an IAM role. An MFA-protected IAM user can be used in a personal sandbox account.
- `AdministratorAccess` simplifies the workshop but is appropriate only for a sandbox. Real environments should use least-privilege permission sets or policies.
- Workshop Region: `ap-southeast-1` (Singapore).

![IAM user in a sandbox account](/images/5-Workshop/5.2-Prerequisites/IAM_Console.png)

> The screenshot shows an IAM user in a sandbox. Do not attach `AdministratorAccess` directly to production workload identities.

## 2. Local development environment

Install and verify:

```bash
node --version
npm --version
aws --version
cdk --version
git --version
```

Current project requirements:

- Node.js 20 or later for workspace builds; the CDK source currently declares the Node.js 20.x Lambda runtime and should be upgraded to a supported runtime before long-lived deployment.
- AWS CLI configured with a profile or temporary credentials.
- AWS CDK CLI v2.
- Git and read access to the `AWS-Project` repository.

Verify the AWS identity before deployment:

```bash
aws sts get-caller-identity
```

## 3. Repository and Amplify

Amplify Console uses the GitHub App to connect a repository. Select the correct repository and the `staging` or `main` branch, recommend `staging`.

The current CDK code reads `GITHUB_REPO_URL`, `GITHUB_BRANCH`, and `GITHUB_TOKEN` from environment variables when creating the Amplify app. Never commit the token to Git or include it directly in documentation.

## 4. Cost and scope

- Use the `staging` environment for this workshop.
- Monitor Billing and Budgets throughout the exercise.
- WAF, log storage, Rekognition, NAT Gateway, and long-running resources can incur charges outside the Free Tier.
- Do not deploy `production` before reviewing `RETAIN` removal policies.
