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

1. Push the tested source to the repository and `staging` branch before opening Amplify.
2. Open the [AWS Amplify Console](https://console.aws.amazon.com/amplify/) in `ap-southeast-1`.
3. Choose **Create new app** → **Host web app** or **Deploy an app**, depending on the current layout.
4. Select **GitHub** as the source provider and choose **Next/Continue**.
5. When prompted, choose **Install & Authorize AWS Amplify** and grant the GitHub App access to the required repository. Use **Only select repositories** to limit scope when possible.
6. Return to Amplify and select repository `AWS-Project` and branch `staging`.
7. Enable monorepo support if shown and enter app root `frontend`. If the build specification below runs from the repository root, inspect the preview to avoid a duplicated `frontend/frontend` path.
8. Choose **Next** to open build settings.

![Connect Amplify through the GitHub App](/images/5-Workshop/5.6-Frontend-Amplify/amplify_app_setup.png)

The screenshot illustrates GitHub App authorization only; select the repository and branch for the target environment.

## 2. Environment variables

1. In **App settings/Build settings**, expand **Environment variables**.
2. Choose **Add variable** and configure four variables:

| Key | Value |
|---|---|
| `VITE_API_URL` | Invoke URL for the `staging` API environment (currently ending in `/dev`) |
| `VITE_USER_POOL_ID` | Cognito User Pool ID |
| `VITE_CLIENT_ID` | Cognito app client ID |
| `VITE_AWS_REGION` | `ap-southeast-1` |

![Frontend environment variable keys](/images/5-Workshop/5.6-Frontend-Amplify/amplify_env_setup.png)

Avoid an unintended trailing `/` in the API base URL. Never place secrets in `VITE_*` variables because their values are embedded in browser-delivered JavaScript.

3. Copy the API URL from API Gateway stage `dev` and the IDs from Cognito rather than inferring them from display names.
4. Verify that the values apply to `staging`. Do not copy production values into the staging branch.
5. After creation, edit values under **Hosting** → **Environment variables** or **App settings** → **Environment variables**, then redeploy the branch.

## 3. Build specification

CDK sets the build specification directly on the Amplify app. The Console equivalent is:

1. Under **Build settings**, choose **Edit** or **Edit YML**.
2. Replace auto-detected content with the configuration below when it does not detect the monorepo correctly.

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

3. Choose **Save** and verify `npm ci`, the `frontend` workspace command, and output directory `frontend/dist`.
4. Do not change `baseDirectory` to `dist` unless app root is already `frontend` and the build log confirms commands run from that directory.
5. Choose **Next**, enter an app name with a `staging` suffix if requested, and review the settings.

## 4. SPA rewrite and deployment

1. Choose **Save and deploy** to start the first build.
2. Open the build and follow **Provision**, **Build**, **Deploy**, and **Verify**. If it fails, inspect the first relevant error in the log before retrying.
3. After deployment, open **Rewrites and redirects** in app settings.
4. Add an SPA-regex source that excludes static files, target `/index.html`, type `200 (Rewrite)`.
5. Do not use `301/302` for React routes; the browser must receive `index.html` while preserving the route URL.
6. Save the rule, open the branch URL, navigate directly to a client-side route, and refresh.
7. In DevTools → Network, verify requests use the `/dev` API URL, protected requests include `Authorization`, and no CORS error occurs.

## 5. Verify automatic builds

1. Push a small change to branch `staging`.
2. Confirm that Amplify starts a new build for the correct branch.
3. Check that the build commit ID matches the pushed commit.
4. If automatic deployment is not wanted after the lab, disable **Auto build**. This is an operational choice and differs from the current CDK default.

> The CloudFront distribution behind Amplify is managed by the service and is not a distribution for the processed S3 bucket. The project's Storage stack still has its separate CloudFront distribution disabled.
