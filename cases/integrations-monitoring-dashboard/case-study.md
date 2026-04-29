# Case: Integrations Monitoring Dashboard

## Context

A large number of automated integrations were running across multiple customer accounts, primarily using Zapier.

There was no centralized visibility into:

- Active vs inactive integrations
- Risk exposure (MRR tied to failing automations)
- Trends over time

## Problem

- Difficult to identify failing or inactive integrations
- No clear prioritization based on business impact
- Data was fragmented across tools (Zapier, internal systems, Jira)
- No historical tracking for trend analysis

## Constraints

- Data needed to be manually extracted on a weekly basis
- Dashboard had to be built using Google Sheets and Looker Studio
- Complexity needed to be handled at the data layer, not in visualization

## My Role

- Designed the data model
- Defined KPIs and risk metrics
- Built the data pipeline in Google Sheets
- Created the dashboard in Looker Studio
- Structured reporting views for operations

## Investigation

I identified that the biggest gap was not data availability, but data structure.

The raw data existed, but:

- It was not normalized
- There was no consistent time-based tracking
- No clear definition of inactive vs active integrations

## Approach

I built a layered data model:

1. Raw data (weekly exports)
2. Cleaned data (normalized fields)
3. Calculated fields (KPIs and risk metrics)

I also introduced time-based snapshots to track changes over time.

## Solution

Created a dashboard with key views such as:

- Total integrations vs relevant integrations
- Inactive integrations over time (7, 14, 30+ days)
- MRR at risk by inactivity bucket
- CRM-related vs non-core integrations
- Weekly trends and distribution

## Architecture / Flow

```text
Data Sources (Zapier / Internal / Jira)
   ↓
Weekly CSV Exports
   ↓
Google Sheets (Source of Truth)
   ↓
Helper Columns & Calculations
   ↓
Looker Studio Dashboard
```

## Impact

- Enabled visibility into integration health
- Quantified business risk tied to automation failures
- Allowed prioritization based on MRR impact
- Created a repeatable reporting process
- Improved operational decision-making

## Tools & Technologies

- Google Sheets
- Looker Studio
- CSV exports
- Data modeling
- KPI design

## Key Learnings

- Data modeling should happen before visualization
- Time-based snapshots are essential for trend analysis
- Simpler dashboards are more effective when data is well-structured
- Operational metrics must connect to business impact (MRR)

## Sanitization Notes

- Real customer data and financial values were generalized
- Internal system names simplified
- Data structures represented conceptually
