---
title: "Week 9 Worklog"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Week 9 Objectives:

* Kick off the official Smart Image Platform project.
* Design the DynamoDB Single-Table schema to optimize performance and indexing costs.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | Analyze the Smart Image Platform requirements and compile the serverless solution architecture diagram. | 06/08/2026 | 06/08/2026 |  |
| 3 | Design the DynamoDB Single-Table schema (Composite keys: PK = USER#<id>, SK = IMG#<timestamp>#<ulid>) for optimal queries. | 06/09/2026 | 06/09/2026 |  |
| 4 | Create S3 raw and processed buckets on the console with Block All Public Access enabled. | 06/10/2026 | 06/10/2026 |  |
| 5 | Provision the main DynamoDB tables: SmartImage-Images, SmartImage-UserQuotas, and SmartImage-UserProfiles in On-demand capacity mode. | 06/11/2026 | 06/11/2026 |  |
| 6 | Practice: Configure GSIs (GSI1-TagIndex-v2, GSI2-ModerationIndex) with projected attributes to optimize query read capacity. | 06/12/2026 | 06/12/2026 |  |

### Week 9 Achievements:

* Successfully provisioned two S3 buckets (`smartimage-raw-bucket` and `smartimage-processed-bucket`) with Block All Public Access enabled, utilizing secure S3 Presigned URLs for client-side uploads.
* Designed the DynamoDB Single-Table composite schema integrating user profiles, quotas, and image metadata across three key tables: `SmartImage-Images`, `SmartImage-UserQuotas`, and `SmartImage-UserProfiles`.
* Configured Global Secondary Indexes `GSI1-TagIndex-v2` and `GSI2-ModerationIndex` with custom attribute projections in On-demand capacity mode to support tag-based and moderation-status queries.
