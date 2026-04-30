# Case: Integrations Monitoring Dashboard

> Designed a hybrid data model combining Zapier inventory data, Salesforce account metadata, and Mixpanel activity windows to monitor integration health and quantify MRR at risk.

## Context

A large number of automated integrations were running across multiple customer accounts, primarily using Zapier.

There was no centralized visibility into:

- Active vs inactive integrations
- Risk exposure (MRR tied to failing automations)
- Trends over time
- Whether inactive automations belonged to core customer-facing flows or non-critical/internal flows

## Problem

- Difficult to identify failing or inactive integrations
- No clear prioritization based on business impact
- Zap inventory data was split across more than one Zapier account
- Activity data lived separately in Mixpanel reports
- Account metadata such as MRR, CS owner, Salesforce ID, and merchant ID lived in Salesforce
- A single account could have multiple zaps with different purposes, which created MRR duplication risk if reporting directly from raw zap-level data
- No consistent model to compare 7-day, 14-day, and 30-day inactivity windows

## Constraints

- The dashboard had to be built using Google Sheets and Looker Studio
- Zapier inventory data came from two account-level sources
- Mixpanel activity data could be pulled into Sheets through a connected extension, but still required a controlled refresh process
- Salesforce account data had to be used for enrichment and MRR context
- Complexity needed to be handled at the data layer, not in visualization

## My Role

- Designed the data model
- Defined KPIs and risk metrics
- Built the data pipeline in Google Sheets
- Extracted account names from zap titles
- Matched account names to Salesforce records to retrieve merchant IDs, MRR, and CS ownership
- Used merchant ID as a cross-reference key for Mixpanel activity windows
- Created unique account logic to avoid duplicate MRR counting
- Created the dashboard in Looker Studio

## Investigation

I identified that the biggest gap was not data availability, but data structure.

The raw zap inventory existed, but it was split across two Zapier account sources. Account metadata existed in Salesforce. Activity signals existed in Mixpanel.

The challenge was joining these layers into a reliable reporting model without inflating MRR or misclassifying inactive automations.

Key findings:

- Zap title was the most practical field for extracting the account name from Zapier inventory data
- Salesforce account data could enrich each integration record with Salesforce ID, merchant ID, MRR, and CS owner
- Merchant ID became a stronger cross-reference key for connecting account records to Mixpanel activity reports
- Raw zap-level reporting could duplicate MRR because one account may have multiple zaps
- A unique-account layer was needed to report account-level exposure accurately

## Approach

I built a layered data model:

1. Zapier Account 1 inventory
2. Zapier Account 2 inventory
3. Salesforce account metadata
4. Mixpanel activity windows (7-day, 14-day, and 30-day activity reports)
5. Account name extraction and normalization
6. Merchant ID cross-reference logic
7. Unique account layer to avoid duplicated MRR
8. Final modeled table for dashboarding
9. Looker Studio reporting views

The model separates zap-level operational monitoring from account-level business risk reporting.

## Solution

Created a dashboard with key views such as:

- Total integrations vs relevant integrations
- Inactive integrations by 7-day, 14-day, and 30-day windows
- MRR at risk by inactivity bucket
- Unique accounts at risk
- CRM-related vs non-core integrations
- CS owner and ownership views
- Failure categories and risk levels

## Architecture / Flow

```text
Zapier Account 1 Inventory        Zapier Account 2 Inventory
          ↓                                ↓
        Combined Zap Inventory (zap-level records)
          ↓
Account Name extracted from Zap Title
          ↓
Salesforce Account Match
          ↓
Merchant ID / MRR / CS Owner enrichment
          ↓
Mixpanel Activity Windows matched by Merchant ID
          ↓
Unique Account Layer to prevent duplicated MRR
          ↓
Final Modeled Table for dashboarding
          ↓
Looker Studio Dashboard
```

## Impact

- Enabled visibility into integration health
- Quantified business risk tied to automation failures
- Allowed prioritization based on MRR impact
- Reduced the risk of inflated MRR caused by duplicate zap-level records
- Created a repeatable refresh and enrichment process inside Google Sheets
- Improved operational decision-making across support and customer success workflows

## Tools & Technologies

- Google Sheets
- Zapier inventory data
- Salesforce account data
- Sheets <> Mixpanel extension
- Mixpanel reports
- Looker Studio
- Data modeling
- KPI design

## Key Learnings

- Data modeling should happen before visualization
- Zap-level records and account-level risk need to be modeled separately
- A stable cross-reference key is essential when joining operational and account data
- Time-based activity windows are essential for integration health monitoring
- Operational metrics must connect to business impact without inflating revenue exposure

## Sanitization Notes

- Real customer data and financial values were generalized
- Internal system names simplified where needed
- Data structures represented conceptually
- Screenshots and raw exports are excluded unless recreated with fictional data
