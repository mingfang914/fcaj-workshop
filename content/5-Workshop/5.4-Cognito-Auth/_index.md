---
title: "Console - Cognito Authentication"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# Amazon Cognito in the AWS Console (optional)

> Skip User Pool creation after deploying the Auth stack. Console layouts can change; preserve the configuration values rather than relying on exact button names.

## 1. Create the User Pool and SPA app client

1. Open the Amazon Cognito Console and create a new application.
2. Select **Single-page application (SPA)** and use a name with the `staging` suffix.
3. Enable email as the only sign-in identifier and enable self-registration.
4. Require the `email` and `name` standard attributes.
5. Enable email verification and email-only account recovery.
6. Use a password policy with a minimum length of 8 and require uppercase, lowercase, digits, and symbols.
7. Set MFA to **Optional**, enable authenticator app/TOTP, and disable SMS.
8. Use a public app client without a client secret and enable SRP authentication.

![SPA, email, and sign-up attribute settings](/images/5-Workshop/5.4-Cognito-Auth/cognito_userpool_setup.png)

## 2. Custom attribute

CDK currently creates a mutable String attribute named `custom:role` with a length of 1–20. It can be created to mirror the stack, but the application does not use it as the primary authorization source.

> **Important difference:** The frontend and backend check `cognito:groups`, not `custom:role`. The custom attribute may therefore be treated as optional in the Console path.

## 3. User groups

Create two groups:

| Group | Description | Precedence |
|---|---|---|
| `admin` | Administrators with moderation access | 0 |
| `user` | Regular users | 10 |

![Project Cognito groups](/images/5-Workshop/5.4-Cognito-Auth/cognito_groups_setup.png)

After a user signs up and verifies email, add the test account to the appropriate group. The project currently has no Post Confirmation trigger that automatically adds new sign-ups to `user`.

Record the User Pool ID and App Client ID for API Gateway and Amplify configuration.
