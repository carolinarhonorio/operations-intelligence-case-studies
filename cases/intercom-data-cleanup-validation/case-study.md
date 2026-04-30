# Case: Intercom Data Cleanup & Salesforce Validation

> Structured a cross-system validation workflow for 16,513 Intercom user records, using Salesforce account data to classify records for deletion, retention, update, duplication review, or investigation.

## Context

An internal support operations environment had a large volume of user records in Intercom that needed to be validated against CRM account data.

The goal was to identify which users should remain active, which records should be deleted, and which records required manual review due to missing or inconsistent account information.

This case focused on improving operational data quality while reducing manual review effort.

## Problem

The user base contained several categories of records:

- Users linked to active accounts
- Users associated with churned accounts
- Duplicate or outdated records
- Test, demo, production, and internal-use accounts
- Records where the email existed in one system but not the other
- Records where the merchant/account name did not match cleanly across systems

A simple exact match was not enough because account names often included inconsistent labels such as churn, test, demo, production, or deletion markers.

## Constraints

- Intercom used email as the primary lookup key.
- Salesforce account data was the reference source for account status.
- Some records required fuzzy or normalized name matching.
- Sensitive customer data could not be exposed in documentation.
- The cleanup process needed to support phased deletion instead of one risky bulk action.

## My Role

I owned the analysis logic and cleanup design, including:

- Defining the cross-system validation rules
- Creating helper columns and classification logic
- Comparing Intercom users against Salesforce account records
- Identifying deletion, update, duplicate, and investigation scenarios
- Manually enriching ambiguous merchant matches where formula-based matching was not enough
- Designing a safer wave-based cleanup strategy

## Investigation

The analysis started by separating the role of each dataset:

- Intercom users represented operational access and support records.
- Salesforce users helped validate email-level presence.
- Salesforce accounts represented account status and customer lifecycle state.
- Email was used as the primary key for user-level validation.
- Merchant/account name was used as a secondary signal when email matching was insufficient.

I also identified that some records could be confidently classified based on terms in the account name, such as churn, test, demo, production, or delete.

## Approach

Instead of treating the cleanup as a simple duplicate-removal task, I modeled it as a data validation workflow.

The logic combined:

1. Email-based matching
2. Salesforce account status lookup
3. Merchant name normalization
4. Churn/test/demo detection
5. Duplicate email detection
6. Action classification
7. Manual enrichment for ambiguous but recognizable merchant names

This made it possible to separate high-confidence deletion candidates from records requiring review.

## Solution

I created a structured spreadsheet workflow with helper fields such as:

- Exists in Intercom?
- Exists in Salesforce?
- Matched Salesforce Account
- Salesforce Account Status
- Suspected Churn/Test/Demo
- Duplicate Email
- Merchant Name Match
- Action Required

The final action logic classified records into categories such as:

- Keep
- Delete
- Update Email
- Review Duplicate
- Investigate
- Manual Review

## Validation Summary

The dataset contained 16,513 total records.

| Metric | Count / Percentage |
|---|---:|
| Total records | 16,513 |
| With email | 12,155 |
| Without email | 4,359 |
| With name only | 3,772 |
| Email only | 1,718 |
| Duplicate emails | 1,761 |
| Suspected churn/test/demo/delete | 9,848 |
| Email match yes | 6,543 |
| Merchant match yes | 13,335 |
| Merchant match rate | 80.75% |
| Email match rate | 53.83% |

## Action Classification

| Action | Count | Share |
|---|---:|---:|
| Delete | 10,475 | 63.43% |
| Keep | 1,214 | 7.35% |
| Investigate | 747 | 4.52% |
| Duplicated | 1,761 | 14.49% |
| Update Email | 1,799 | 14.80% |
| Review | 1,570 | 9.51% |

## Before vs After Enrichment

After the initial formula-based cross-reference, I manually enriched recognizable merchant matches that formulas could not confidently resolve.

This reduced ambiguous cases and increased the number of records with a clear next action.

| Metric | Before | After | Change |
|---|---:|---:|---:|
| Merchant Match | 12,303 | 13,240 | +937 |
| Merchant Match Rate | 74.50% | 80.18% | +5.68 p.p. |
| Delete | 9,646 | 10,324 | +678 |
| Delete Rate | 58.41% | 62.52% | +4.11 p.p. |
| Investigate | 1,552 | 879 | -673 |
| Review | 1,953 | 1,584 | -369 |

The enrichment step significantly reduced the number of unclear records and increased the first cleanup wave confidence.

## Architecture / Flow

```text
Intercom Users
   ↓
Email-based validation
   ↓
Salesforce user lookup
   ↓
Salesforce account lookup
   ↓
Account status enrichment
   ↓
Merchant name normalization
   ↓
Churn/test/demo/delete detection
   ↓
Duplicate email detection
   ↓
Formula-based action classification
   ↓
Manual enrichment for ambiguous merchant matches
   ↓
Wave-based cleanup plan
```

## Impact

- Created a repeatable framework for validating 16,513 user records across systems
- Classified 10,475 records for deletion with a clear rule-based rationale
- Reduced investigation cases from 1,552 to 879 after manual enrichment
- Increased merchant match coverage by 937 records
- Separated high-confidence cleanup actions from ambiguous cases
- Improved visibility into stale, churned, demo, and duplicate records
- Turned a manual cleanup effort into a structured operational workflow

## Tools & Technologies

- Intercom
- Salesforce
- Google Sheets
- Lookup formulas
- Regex-based classification
- Data normalization
- Operational data QA

## Key Learnings

- The correct primary key depends on the system and business context.
- Exact name matching is often unreliable in operational datasets.
- Manual enrichment can be valuable when used after structured rule-based classification, not instead of it.
- Cleanup workflows should separate confidence levels before taking action.
- A good data cleanup process is not just about deleting records; it is about protecting valid operational access while reducing noise.

## Sanitization Notes

- Customer names were removed or generalized.
- Real account names, emails, and identifiers were excluded.
- Internal URLs and exports were not included.
- Examples were rewritten as generic operational patterns.
