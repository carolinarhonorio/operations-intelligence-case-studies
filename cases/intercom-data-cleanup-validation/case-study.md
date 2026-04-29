# Case: Intercom Data Cleanup & Salesforce Validation

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
- Designing a safer wave-based cleanup strategy

## Investigation

The analysis started by separating the role of each dataset:

- Intercom users represented operational access and support records.
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
5. Action classification

This made it possible to separate high-confidence deletion candidates from records requiring review.

## Solution

I created a structured spreadsheet workflow with helper fields such as:

- Exists in Intercom?
- Exists in Salesforce?
- Matched Salesforce Account
- Salesforce Account Status
- Suspected Churn/Test/Demo
- Merchant Name Match
- Action Required

The final action logic classified records into categories such as:

- Keep
- Delete
- Update Email
- Review Duplicate
- Investigate
- Manual Review

## Architecture / Flow

```text
Intercom Users
   ↓
Email-based validation
   ↓
Salesforce account lookup
   ↓
Account status enrichment
   ↓
Merchant name normalization
   ↓
Churn/test/demo detection
   ↓
Action classification
   ↓
Wave-based cleanup plan
```

## Impact

- Created a repeatable framework for validating user records across systems
- Reduced the risk of deleting valid users
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
- Cleanup workflows should separate confidence levels before taking action.
- A good data cleanup process is not just about deleting records; it is about protecting valid operational access while reducing noise.

## Sanitization Notes

- Customer names were removed or generalized.
- Real account names, emails, and identifiers were excluded.
- Internal URLs and exports were not included.
- Examples were rewritten as generic operational patterns.
