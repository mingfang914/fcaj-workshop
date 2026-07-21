---
title: "Console - Amplify Hosting"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

# React Frontend on AWS Amplify (optional)

> The Frontend stack creates the Amplify app, branch, build specification, environment variables, and SPA rewrite through CDK. This section shows the Console equivalent.

## 1. Connect the repository

1. Open the Amplify Console, choose **Create new app**, and select **GitHub**.
2. Install/authorize the AWS Amplify GitHub App for the required repository. No Personal Access Token value or screenshot is needed in the Console path.
3. Select the `AWS-Project` repository and the `staging` branch.
4. Mark the application as a monorepo if the Console presents that option.

![Connect Amplify through the GitHub App](/images/5-Workshop/5.6-Frontend-Amplify/amplify_app_setup.png)

The screenshot illustrates GitHub App authorization only; select the repository and branch for the target environment.

## 2. Environment variables

Configure four variables:

| Key | Value |
|---|---|
| `VITE_API_URL` | Invoke URL for the `staging` API environment (currently ending in `/dev`) |
| `VITE_USER_POOL_ID` | Cognito User Pool ID |
| `VITE_CLIENT_ID` | Cognito app client ID |
| `VITE_AWS_REGION` | `ap-southeast-1` |

![Frontend environment variable keys](/images/5-Workshop/5.6-Frontend-Amplify/amplify_env_setup.png)

Avoid an unintended trailing `/` in the API base URL. Never place secrets in `VITE_*` variables because their values are embedded in browser-delivered JavaScript.

## 3. Build specification

CDK sets the build specification directly on the Amplify app. The Console equivalent is:

```yaml
version: 1
frontend:
  phases:
    preBuild:
      commands:
        - npm ci
    build:
      commands:
        - npm run --workspace=frontend build
  artifacts:
    baseDirectory: frontend/dist
    files:
      - '**/*'
  cache:
    paths:
      - node_modules/**/*
```

When the repository uses `amplify.yml`, keep it equivalent to the build specification above; avoid maintaining two conflicting sources of build configuration.

## 4. SPA rewrite and deployment

Add a `200` rewrite to `/index.html` for client-side routes while excluding static files such as CSS, JS, PNG, SVG, fonts, maps, and JSON. Choose **Save and deploy**, then verify the `staging` branch URL.

> The CloudFront distribution behind Amplify is managed by the service and is not a distribution for the processed S3 bucket. The project's Storage stack still has its separate CloudFront distribution disabled.
