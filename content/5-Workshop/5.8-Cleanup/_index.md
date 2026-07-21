---
title: "Resource Cleanup"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---

# Resource Cleanup

Delete resources only from the intended account, Region, and environment. Do not destroy production before reviewing data-retention requirements.

## A. CDK-managed resources

From the `AWS-Project` root, verify the `staging` stack list:

```bash
npm run --workspace=infrastructure cdk -- list -c environment=staging
```

Then run:

```bash
npm run --workspace=infrastructure destroy -- -c environment=staging
```

In `staging`, both S3 buckets use `autoDeleteObjects` and `DESTROY`, so CDK can empty and remove them during destruction. Manual emptying is unnecessary unless the deployment was changed or the custom resource fails.

After destruction, inspect CloudFormation and the relevant services to verify that all six stacks were deleted.

## B. Resources created manually in the Console

`cdk destroy` does not remove resources created separately in the Console. Delete them in dependency-aware order:

1. Amplify app and branch.
2. WAF association, then the Web ACL.
3. API Gateway stage/API and API access log group.
4. S3 Event Notification, DynamoDB event source mapping, and Lambda triggers.
5. Lambda functions and related log groups.
6. Both SQS DLQs.
7. DynamoDB tables after reviewing backup/PITR requirements.
8. Empty and delete raw/processed buckets, including object versions when versioning was enabled.
9. Cognito app client, groups, and User Pool.
10. CloudWatch dashboard, alarms, SNS subscriptions, and topic.
11. IAM roles and custom policies created only for the workshop.

## C. Post-cleanup verification

- Recheck `ap-southeast-1` and any other Region used during the workshop.
- Search for resources with the `SmartImage` prefix/tag and `staging` environment.
- Inspect CloudFormation stacks in `DELETE_FAILED`.
- Check S3 object versions, CloudWatch Logs, SQS, SNS, WAF, and Amplify.
- Monitor Billing/Cost Explorer over the following days because cost data can be delayed.

> Production applies `RETAIN` to selected buckets, tables, and the User Pool. `cdk destroy` does not remove retained resources. Do not claim zero cost until Billing confirms it and no out-of-stack resources remain.
