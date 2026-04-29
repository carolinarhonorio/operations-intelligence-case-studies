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
- Data was fragmented across tools such as Zapier, Mixpanel, internal account data, and operational tracking sheets
- No consistent model to compare 7-day, 14-day, and 30-day inactivity windows
- Limited historical tracking for trend analysis

## Constraints

- The dashboard had to be built using Google Sheets and Looker Studio
- Mixpanel activity data could be pulled into Sheets through a connected extension, but still required a controlled refresh process
- Some account and ownership data needed to be maintained or normalized separately
- Complexity needed to be handled at the data layer, not in visualization

## My Role

- Designed the data model
- Defined KPIs and risk metrics
- Built the data pipeline in Google Sheets
- Connected Mixpanel reports into Sheets using the Sheets <> Mixpanel extension
- Structured 7-day, 14-day, and 30-day inactivity views
- Created the dashboard in Looker Studio
- Structured reporting views for operations

## Investigation

I identified that the biggest gap was not data availability, but data structure.

The raw activity data existed in Mixpanel, and account/business context existed in other operational sources, but:

- The data was not normalized for reporting
- Activity windows needed to be compared consistently
- Integration relevance needed to be separated from overall automation volume
- Account ownership and MRR context needed to be joined to integration activity
- Dashboard logic would become fragile if too much calculation lived directly in Looker Studio

## Approach

I built a layered data model:

1. Mixpanel activity reports synced into Google Sheets
2. Supporting account and integration metadata
3. Cleaned and normalized fields
4. Helper columns for 7-day, 14-day, and 30-day inactivity logic
5. Calculated KPIs and risk metrics
6. Looker Studio reporting views

The refresh process became partially automated: Mixpanel reports were configured and could be refreshed directly inside Google Sheets using the extension's sync process, instead of relying only on manual CSV exports.

## Solution

Created a dashboard with key views such as:

- Total integrations vs relevant integrations
- Inactive integrations over time (7, 14, 30+ days)
- MRR at risk by inactivity bucket
- CRM-related vs non-core integrations
- Weekly trends and distribution

## Architecture / Flow

```text
Mixpanel Reports (7d / 14d / 30d activity windows)
   ↓
Google Sheets <> Mixpanel Extension (Sync Now refresh)
   ↓
Google Sheets Source of Truth
   ↓
Account / MRR / Ownership Enrichment
   ↓
Helper Columns & KPI Calculations
   ↓
Looker Studio Dashboard
```

## Impact

- Enabled visibility into integration health
- Quantified business risk tied to automation failures
- Allowed prioritization based on MRR impact
- Reduced reliance on fully manual CSV exports
- Created a repeatable refresh process inside Google Sheets
- Improved operational decision-making

## Tools & Technologies

- Google Sheets
- Sheets <> Mixpanel extension
- Mixpanel reports
- Looker Studio
- Zapier activity context
- Data modeling
- KPI design

## Key Learnings

- Data modeling should happen before visualization
- A dashboard is stronger when refresh logic is controlled and repeatable
- Time-based activity windows are essential for integration health monitoring
- Simpler dashboards are more effective when data is well-structured
- Operational metrics must connect to business impact (MRR)

## Sanitization Notes

- Real customer data and financial values were generalized
- Internal system names simplified where needed
- Data structures represented conceptually
- Screenshots and raw exports are excluded unless recreated with fictional data
