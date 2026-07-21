---
title: "Week 3 Worklog"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

* Study Amazon S3 object storage configurations and data management optimization.
* Learn Amazon CloudFront CDN to distribute static web assets securely and with high performance.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | Study Amazon S3 basics: storage classes (Standard, IA, Glacier), object upload/download, and metadata management. | 04/27/2026 | 04/27/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 - 4 | Configure S3 Bucket Versioning to store file history. Set up Lifecycle Rules to automatically transition objects to Glacier and configure Cross-Region Replication (CRR). | 04/28/2026 | 04/29/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | Set up S3 static website hosting. Write bucket policies to allow public read access for assets. | 04/30/2026 | 04/30/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 - 6 | Study Amazon CloudFront CDN: Edge locations, caching behaviors, TTL management, and HTTPS configurations using AWS Certificate Manager (ACM). | 04/30/2026 | 05/01/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | Practice the CloudFront model separately with an S3 REST endpoint: keep the bucket private, enable Block Public Access, and configure Origin Access Control (OAC) for object access. | 05/01/2026 | 05/01/2026 | https://cloudjourney.awsstudygroup.com/ |

### Week 3 Achievements:

* Uploaded objects, managed metadata, enabled Versioning, and inspected Lifecycle Rules on the lab bucket.
* Configured Cross-Region Replication (CRR) between two buckets and observed object replication to the destination Region.
* Distinguished the two delivery models: an S3 website endpoint requires public read access, whereas CloudFront OAC uses a private S3 REST origin.
* Kept the OAC lab bucket private and accessed its content through CloudFront. CloudFront is not enabled in the current Smart Image project; the application currently returns S3 presigned URLs.
