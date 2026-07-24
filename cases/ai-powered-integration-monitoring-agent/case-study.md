# Case: AI-Powered Integration Monitoring Agent

> Designed and implemented an autonomous AI monitoring workflow that combines Google Drive, Mixpanel, and Slack to detect inactive customer integrations, prioritize operational risk, and generate executive-ready daily reports without manual intervention.

## Context

Our integrations ecosystem contains hundreds of customer automations built across multiple CRMs and workflows.

Although an operational dashboard already existed, monitoring still required someone to:

- Refresh reports
- Cross-reference multiple systems
- Interpret inactivity
- Decide which accounts deserved attention
- Summarize findings for the team

The reporting process was repetitive and depended on manual analysis every day.

## Problem

The challenge wasn't collecting data.

The challenge was turning several operational datasets into a daily decision-making process.

The monitoring workflow needed to:

- Read the current integration inventory
- Retrieve activity from Mixpanel
- Classify inactivity
- Understand ownership
- Prioritize operational actionability
- Produce a concise Slack report

without requiring human intervention.

## Constraints

Several technical constraints shaped the solution:

- Google Drive connector truncates large spreadsheets
- Mixpanel exports are large daily matrices
- Multiple systems use different identifiers
- Ownership does not equal priority
- Slack messages have practical reading limits
- The workflow needed to run autonomously in the cloud

## My Role

I designed both the operational logic and the AI workflow.

Responsibilities included:

- Defining business rules
- Designing the monitoring architecture
- Creating the prompt specification
- Defining ownership semantics
- Designing risk classification
- Optimizing token usage
- Iterating report formatting
- Building an autonomous scheduled workflow

## Investigation

The biggest discovery was that AI performance depended much more on architecture than prompting.

Initial attempts exposed the entire spreadsheet to the model.

This caused:

- Unnecessary context
- Hallucinated alerts
- Poor prioritization
- Long Slack messages

Rather than improving prompts, I redesigned the data pipeline.

## Solution

The final architecture separates responsibilities into distinct layers.

### Data Layer

A curated Google Sheet defines the monitoring universe.

Only active integrations are exposed.

The spreadsheet itself becomes the source of truth.

### Activity Layer

Mixpanel provides activity windows.

Merchant IDs become the common reference key.

### Business Logic

Risk is determined by inactivity.

Priority is determined by operational ownership.

These are treated as independent dimensions.

Support accounts surface first because they are fully actionable.

Shared workflows come afterwards.

Client-owned workflows appear last.

### AI Layer

Claude receives:

- Curated integration inventory
- Mixpanel activity
- Business rules
- Ownership definitions
- Reporting instructions

The model performs reasoning only.

It does not decide business rules.

### Report Design

Several iterations reduced noise while preserving information.

The final report follows this hierarchy:

Executive Summary

↓

Risk Tier

↓

Ownership

↓

Exceptions

↓

Collapsed operational patterns

This reduced the report by almost 50% while improving scanability.

## Architecture / Flow

```text
Google Drive
        │
        ▼
Curated Monitoring Sheet
        │
        ├─────────────┐
        ▼             ▼
 Mixpanel       Business Rules
        │             │
        └──────┬──────┘
               ▼
         Claude Sonnet 5
               │
        Risk Classification
               │
        Executive Summary
               │
               ▼
            Slack
```

## Impact

The workflow now:

- Runs autonomously every day
- Requires no manual spreadsheet analysis
- Produces executive-ready summaries
- Prioritizes accounts by operational actionability
- Reduces investigation time
- Standardizes operational decision-making

## Key Learnings

The project reinforced that:

- AI quality depends more on system architecture than prompt length
- Separating business rules from reasoning dramatically improves consistency
- Curated context outperforms exposing raw operational data
- Ownership and risk are different dimensions and should never be conflated
- Executive communication benefits from hierarchical summarization rather than exhaustive reporting

## Tools & Technologies

- Claude Sonnet 5
- Google Drive
- Mixpanel
- Slack
- Google Sheets
- Prompt engineering
- AI workflow design
- Operational intelligence
- Data modeling
- Risk classification

## Sanitization Notes

All customer names, MRR values, identifiers, workflows, and operational data have been anonymized or generalized. The architecture and decision-making process accurately represent the production implementation while protecting confidential business information.
