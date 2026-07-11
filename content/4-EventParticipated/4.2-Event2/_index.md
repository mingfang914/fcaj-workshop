---
title: "Event 2"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Summary Report: "FCAJ Community Day 23/5"

### Event Objectives

- Provide deep technical insights and real-world experiences regarding AWS Cloud infrastructure optimization, Generative AI integration, and edge security.
- Facilitate networking and technical knowledge exchanges between industry practitioners and the First Cloud AI Journey community.
- Direct design choices toward scalable, enterprise-grade AI architectures, data security, and operational cost savings.

### Speakers

- **Tinh Truong** – Platform Engineer at GoTyme Bank (Topic: "*Context Is Everything*")
- **Phạm Ngọc Hải Anh** – AWS Community Builder at G-AsiaPacific Vietnam (Topic: "*Friendly AI Assistant w/ Amazon Q*")
- **Nguyễn Tuấn Thịnh** – DevOps Engineer (Topic: "*From Edge To Origin: CloudFront as Your Foundation*")
- **Team VIB** – Presenters from LotusHacks 2026 (Topic: "*36 hrs with LotusHacks – Building UTMorpho from Idea to Reality*")
- **Đức Đào** – Solution Architect at Cloud Kinetics (Topic: "*Non-Determinism of 'Deterministic' LLM Settings*")
- **Vy Lâm** – Senior Business Systems Analyst at VPBank (Topic: "*Enterprise-Grade Multi-Agent System: The Case of Startup Credit Scoring*")

### Key Highlights

#### 1. The Critical Role of Context in LLMs (Tinh Truong)
- Addressed the misconception that poor AI output stems from weak models, showing that poor context quality is usually the bottleneck.
- Identified three common pitfalls: dumping raw material indiscriminately, repeating obvious facts the model already knows, and providing prompts devoid of constraints.
- Proposed a 4-element context engineering framework: Goal, Relevant info, Constraints, and Success criteria. Introduced Obsidian/Notion-based Second Brain systems to optimize context and memory.

#### 2. Building Intelligent Assistants with Amazon Q (Phạm Ngọc Hải Anh)
- Demonstrated Amazon Q's ability to ingest and process unstructured information from diverse data connectors (world knowledge, company spaces, local files) using Amazon Bedrock.
- Outlined a practical PM Assistant use case that automates the generation of Minutes of Meetings (MoM), drafts stakeholder emails, and schedules follow-up actions.

#### 3. Content Delivery and Edge Security via Amazon CloudFront (Nguyễn Tuấn Thịnh)
- Analyzed CloudFront's global edge caching architecture and volumetric DDoS protection leveraging AWS Shield and AWS WAF.
- Explained cost reduction patterns such as free data transfer from AWS origins to CloudFront, reducing origin server CPU overhead (from 5% to 1%) by offloading TLS handshakes and compressing assets (82% size reduction with gzip/brotli).
- Discussed Origin Cloaking configurations using Origin Access Control (OAC) for S3 buckets/Lambda and VPC Origins for private ALBs.

#### 4. Practical Hackathon Collaboration (Team VIB)
- Shared experiences from a 36-hour sprint developing UTMorpho at the LotusHacks 2026 hackathon.
- Discussed resolving issues like LLM overgeneration, token limits, and scope management to deliver an MVP under pressure.

#### 5. Deconstructing the Non-Determinism of "Deterministic" LLMs (Đức Đào)
- Deconstructed next-token selection using logit probability distributions and the roles of Temperature, Top-P, and Top-K parameters.
- Explained why `temperature=0` fails to guarantee reproducibility in production, citing floating-point non-associativity in GPU computations and dynamic request batching on inference servers.
- Recommended majority voting across parallel runs and setting `temperature=0.1` as a production-stable sweet spot.

#### 6. Multi-Agent Systems in Startup Credit Scoring (Vy Lâm)
- Examined the data mismatch between traditional banking frameworks and startup metrics (burn rate, runway, unit economics).
- Compared single-agent limits (context limits, expertise dilution, lack of checks and balances) with a Virtual Credit Committee blueprint using collaborating agents (Manager, Financial Analyst, Market Analyst).
- Outlined a deployment flow starting with CrewAI locally, packaged as Docker containers on Amazon ECR, and executed via Bedrock Agent runtimes.

### Lessons Learned

- **Context Quality:** Shifted focus from increasing data quantity to engineering precise context for LLMs.
- **Edge Caching & Security:** Mastered the integration of Amazon CloudFront and S3 OAC to safeguard static resources.
- **LLM Inferencing Dynamics:** Understood the underlying hardware and software constraints that cause non-deterministic behavior on GPUs.
- **Multi-Agent Design:** Gained knowledge on orchestrating multiple specialized AI agents under enterprise governance constraints.

### Applying to Work

- Configured **Amazon CloudFront combined with OAC** to secure image delivery for the processed S3 bucket in the Smart Image Platform project.
- Tuned Bedrock API invocations to **Temperature = 0.1** to ensure reliable, structured data outputs.
- Established a **Second Brain** document repository to manage the internship files systematically.

### Event Experience

- A highly informative event combining technical presentations with practical, live demos.
- Provided a valuable forum to engage with DevOps, Platform, and AI engineers working at scale.

#### Event Photos
![Event photos](/images/4-EventParticipated/event2-1.png)
![Event photos](/images/4-EventParticipated/event2-2.png)
![Event photos](/images/4-EventParticipated/event2-3.png)
