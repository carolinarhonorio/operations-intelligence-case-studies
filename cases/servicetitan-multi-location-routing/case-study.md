# Case: Multi-location CRM Routing via Zapier

## Context
A multi-location service business needed to automate review requests based on completed jobs coming from their CRM.

Each location operated under a different business unit, and review requests needed to be routed correctly per location.

## Problem
The CRM sends job completion events, but:
- Multiple locations exist under the same account
- Incorrect routing could trigger review requests for the wrong location
- No native integration handled this logic

## Constraints
- CRM structure defines locations via "business units"
- Integration relies on Zapier
- OneLocal requires correct location context for review requests
- No room for duplicate or incorrect messaging

## My Role
- Owned integration setup end-to-end
- Defined routing logic
- Built and tested automation
- Coordinated with the customer

## Investigation
- Analyzed CRM payload structure
- Identified business unit as the key routing field
- Validated available triggers (completed job only)
- Confirmed how location mapping works downstream

## Approach
Instead of trying to map based on names or labels, I used the business unit ID as the single source of truth.

This ensured consistency and avoided ambiguity across locations.

## Solution
Built a Zapier workflow:
- Trigger: Completed Job from CRM
- Filter/Paths: Based on Business Unit ID
- Action: Send review request to correct location

## Architecture / Flow

CRM → Zapier Trigger → Filter (Business Unit)
→ Route A → Location A
→ Route B → Location B
→ OneLocal Review Request

## Impact
- Eliminated manual routing
- Ensured accuracy per location
- Enabled scalable automation for multi-location setups
- Created a reusable integration pattern

## Tools & Technologies
- Zapier
- CRM (ServiceTitan)
- Webhooks / API logic
- OneLocal platform

## Key Learnings
- Always prioritize stable identifiers over labels
- Multi-location systems require strict routing logic
- Automation must mirror real system behavior to be reliable

## Sanitization Notes
- Business names replaced
- Business Unit IDs masked
- Internal systems described generically
