---
title: "Week 9 Worklog"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Week 9 Objectives:

* Kick off the official Smart Image Platform project.
* Define access patterns and design DynamoDB data storage for image metadata, quotas, and user profiles.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | Analyze the Smart Image Platform requirements, identify the upload, processing, analysis, and query flows, and update the serverless architecture diagram. | 06/08/2026 | 06/08/2026 | `AWS-Project` source and project documentation |
| 3 - 4 | Design the `Images` table composite key (`PK = USER#<id>`, `SK = IMG#<timestamp>#<ulid>`) from the application's access patterns. | 06/09/2026 | 06/10/2026 | `infrastructure/lib/stacks/database-stack.ts` |
| 4 | Declare raw and processed buckets in CDK with Block Public Access and SSE-S3 encryption enabled. | 06/10/2026 | 06/10/2026 | `infrastructure/lib/stacks/storage-stack.ts` |
| 5 | Declare the `Images`, `UserQuotas`, and `UserProfiles` DynamoDB tables in On-demand mode with environment-specific names. | 06/11/2026 | 06/11/2026 | `infrastructure/lib/stacks/database-stack.ts` |
| 5 - 6 | Configure `GSI1-TagIndex-v2` for tag searches and `GSI2-ModerationIndex` for the moderation queue. | 06/11/2026 | 06/12/2026 | `infrastructure/lib/stacks/database-stack.ts` |

### Week 9 Achievements:

* Provisioned two private S3 buckets through CDK. The client uploads through presigned URLs; the application currently returns S3 presigned URLs for reads because CloudFront is not enabled.
* Used three DynamoDB tables: `Images`, `UserQuotas`, and `UserProfiles`. Only `Images` applies a single-table pattern to image metadata and related records; quotas and profiles remain separate by responsibility.
* Added the `staging` or `production` suffix to table names. CDK adds a unique component to physical S3 bucket names.
* Checked both GSI access patterns: searching images by tag and listing images awaiting moderation.
