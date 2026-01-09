---
title: "What $4.5M/Year in Cloud Savings Actually Required"
description: "The reality behind cloud cost optimization - strategy, engineering work, and organizational alignment."
date: 2024-01-09
slug: "cloud-cost-savings"
categories:
  - Cloud Cost / FinOps
tags:
  - aws
  - gcp
  - finops
  - cloud-migration
  - cost-optimization
draft: false
---

Everyone wants to save money on cloud infrastructure. But the gap between "we should optimize costs" and "$4.5M/year in savings" involves real engineering work, strategic decisions, and organizational alignment.

Here's what it actually took to achieve that outcome during a GCP → AWS migration.

## Context

**Migration Scope:**
- 200+ microservices across multiple Kubernetes clusters
- Multi-region production workloads with strict SLOs
- GCP → AWS over 12 months
- Zero-downtime requirement

**Cost Reduction Target:** ~$4.5M/year in cloud spend

## What Actually Drove Savings

The savings came from **three strategic pillars**, not just "rightsizing instances."

### 1. Architectural Changes (40% of savings)

**Compute consolidation:**
- Moved from over-provisioned GKE clusters to AWS EKS with Karpenter autoscaling
- Adopted Fargate for batch workloads (no idle capacity cost)
- Migrated stateless services to Lambda where appropriate (5+ services)

**Data platform optimization:**
- Replaced expensive managed services with self-hosted alternatives where TCO justified it
- Consolidated redundant databases (7 → 3 data stores)
- Implemented lifecycle policies for S3 (95% of data moved to Glacier after 90 days)

### 2. Commitment Strategy (35% of savings)

**AWS Savings Plans:**
- 1-year compute savings plans for predictable workloads
- Reserved capacity for RDS and ElastiCache
- Strategic timing: committed after stabilization period (month 6)

**Why this mattered:**
- GCP didn't offer equivalent flexibility for our workload patterns
- AWS commitment model aligned better with our growth trajectory
- Avoided over-committing by analyzing 6 months of actual usage first

### 3. Operational Efficiency (25% of savings)

**Automated rightsizing:**
- Built internal tooling to analyze Kubernetes resource requests vs actual usage
- Reduced over-provisioning from 60% to 15% across clusters
- Implemented VPA (Vertical Pod Autoscaler) for 80% of services

**Observability-driven optimization:**
- Used Datadog metrics to identify idle resources and low-utilization services
- Established cost attribution per team/service via tags
- Created FinOps dashboards showing cost per deploy, per request, per customer

## What Made This Possible

This wasn't just a FinOps spreadsheet exercise. It required:

**Engineering Investment:**
- 6 months of dedicated platform team time (3-4 engineers)
- Migration tooling, IaC refactoring, observability improvements
- New autoscaling and resource management systems

**Organizational Alignment:**
- Executive sponsorship for migration timeline
- Cross-functional working groups (SRE, Platform, App teams)
- Clear accountability: each team owned their service cost targets

**Risk Management:**
- Parallel run for critical services (both clouds for 4 weeks)
- Progressive rollout by service tier (non-critical → critical)
- Rollback plans for every phase

## Lessons Learned

**What worked:**
- **Data-driven decisions** - 6 months of profiling before commitment purchases
- **Team ownership** - Cost visibility per service motivated optimization
- **Incremental approach** - 12-month timeline allowed course correction

**What was harder than expected:**
- **Stateful service migration** - Required 3x the estimated time
- **Organizational change** - Engineering teams needed education on cloud cost models
- **Measurement** - Attributing savings to specific initiatives took custom tooling

**What I'd do differently:**
- Start FinOps culture earlier (before migration)
- Invest in cost observability tooling upfront
- Set clearer SLOs for cost per unit metrics

## The Real Cost of Cost Savings

Cloud cost optimization isn't free. Our investment:

- **Engineering time:** ~18 engineer-months over 12 months
- **Tooling:** ~$50K in additional observability and FinOps tooling
- **Risk:** Migration risk, potential downtime, team context-switching

**ROI:** $4.5M annual savings / $500K investment = **9x first-year return**

## Takeaways for Platform Teams

If you're tackling cloud cost optimization:

1. **Start with observability** - You can't optimize what you don't measure
2. **Align incentives** - Make cost visibility part of team dashboards
3. **Combine strategy + execution** - Architecture changes + commitments + rightsizing
4. **Invest in automation** - Manual optimization doesn't scale
5. **Think TCO, not just cloud bills** - Include engineering time, tooling, opportunity cost

---

Cloud cost optimization is a platform engineering problem, not just a procurement exercise. The savings came from treating it as a multi-quarter strategic initiative with dedicated resources and clear accountability.

**Want to discuss cloud cost strategies?** [Email me](mailto:eythandecker@gmail.com) or connect on [LinkedIn](https://www.linkedin.com/in/eythandecker/).
