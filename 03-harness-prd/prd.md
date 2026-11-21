# AI PRD · Juno

> Module 3 · Write an AI PRD Engineers Can Build from. The AI product requirements doc specifying all six harness surfaces, built with the **M3 · AI PRD Builder** (archetype and starting settings from the **M3 · Harness Architecture Decider**). Paste the tool's markdown over this file.

## Problem & user

_The user problem and who has it._

**Problem:** Product Managers need to turn scattered customer feedback into a concise, evidence-based weekly summary for executive review. Doing this manually takes time, and recurring themes or important individual issues can be hard to spot.

**User:** The Product Manager preparing the weekly product review. Executives are the audience for the summary, not the primary user of Juno.____

## The harness

| Surface | Specification |
|---|---|
| 01 Context | _what Juno gets to see_ |
| 02 Tools | _the verb list_ |
| 03 Loop | _turn ceiling and escalation_ |
| 04 Memory | _what persists, at what scope_ |
| 05 Permissions | _read … · draft … · write … · send …_ |
| 06 Verification | _the check before output ships_ |

## 01 Context · Data Requirements

- **Required sources:** comments and optional period/customer count entered by the PM."_
- **Deliberate exclusions:** :

- Customer names, contact details, and other personal information. Juno only needs the feedback text.
- Feedback or company information that the PM hasn’t provided or explicitly selected.
- Product analytics, support tickets, sales notes, and other systems that aren’t part of the agreed input.
- Assumptions about root causes, customer segments, or overall trends that the supplied feedback doesn’t support.
- Roadmap plans or internal strategy documents unless they’re intentionally added as context for the review.

This keeps the summary grounded in the feedback batch. If a relevant RocketShip document changes what Juno should see, add that source deliberately rather than making it available by default._

- **SLA of truth:**  3 months._
- **When a source is unreachable:** _Ask PM to provide the source_

## 02 Tools · System Capabilities

- **The verb list:** For Juno’s weekly feedback summary, list the **tools Juno needs**, the **systems it can access**, and the action type for each. Keep prototype capabilities separate from future integrations.

### Tools and system capabilities

| Capability | What it does | Side-effect class |
|---|---|---|
| Read supplied feedback | Accepts comments pasted by the PM, plus optional period and customer count | **Read** |
| Count comments | Counts submitted comments; reports customer count only if supplied | **Read** |
| Group feedback | Identifies candidate positive and negative themes and keeps unmatched comments visible | **Draft** |
| Draft review summary | Produces positive feedback, negative feedback, key takeaway, and follow-up to consider, with supporting comments | **Draft** |
| Check draft | Checks that counts match the input and each theme is supported by supplied comments | **Read** |
| Copy summary | Copies the draft when the PM selects “Copy” | **Write** |

**Systems:** In this prototype, the only input system is the Juno form; there are no connections to CRM, support, analytics, email, or other company systems.

- **Deliberate omissions:** No web search, external data retrieval, editing source feedback, saving to a company system, or sending/publishing the summary. Those actions aren’t needed for the prototype and could introduce unsupported context or share an unreviewed draft._

## 03 Loop · AI Costs & Latency

- **Turn ceiling:** _5 turns per request._
- **Escalation trigger:** Juno should stop and flag the draft for Product Manager review when it can’t produce a reliable, evidence-grounded summary—for example, when no feedback is provided, comments are ambiguous or contradictory, a theme has no clear supporting comments, or the reported counts don’t match the supplied input.
Juno can still show a draft with the uncertainty clearly marked, but it should not present that draft as ready to share._
- **Latency and cost target:** _For this first-pass product brief, set a **provisional target** of:

- **Latency:** Summary ready within **10 seconds** for up to 100 feedback comments.
- **Cost:** Keep model and processing cost below **$0.10 per summary**._

## 04 Memory · Data Requirements
## Data requirements

- **Required input:** Customer feedback supplied by the Product Manager. Each comment should remain identifiable within the batch so Juno can connect themes and counts back to evidence.
- **Optional input:** Review period and customer count. If either is missing, Juno should say so rather than infer it.
- **Source of truth:** The supplied feedback is authoritative for the summary. Juno should not silently add information from other systems.
- **Freshness:** The summary reflects the batch provided at run time. There’s no automatic refresh in this prototype.
- **Missing input:** If no feedback is supplied, Juno should ask for it and not generate a summary.

## Memory requirements

- Keep feedback and draft content **only for the current request/session**.
- Don’t retain comments, customer details, or inferred themes across sessions.
- Don’t write feedback or summaries to persistent storage. The PM’s source data and corrections take precedence over Juno’s interpretation.
- **What persists, at what scope:** _**Nothing persists beyond the current request.** Juno uses the supplied comments, review period, and customer count only to create that session’s summary. It doesn’t retain them across sessions or at team scope._
- **Expiry:** Feedback and draft summary expire when the session ends. Juno doesn’t retain them afterward.._
- **Write rules:** Juno does not write feedback, themes, or summaries to persistent storage. It may copy the draft to the clipboard only when the Product Manager selects **Copy summary**. If the PM corrects a theme or count, their correction overrides Juno’s draft for the current session.._

## 05 Permissions · AI Risks & Mitigations

| Side-effect class | Tier | Scope |
|---|---|---|
| **Read** | Auto | Read only the feedback and optional context the Product Manager supplies for this request. |
| **Draft** | Auto | Create a review summary, clearly labeled as a draft. |
| **Write** | Confirm | Copy the summary only when the Product Manager selects **Copy summary**. Persistent writes are blocked. |
| **Send** | Blocked | Juno cannot email, publish, or share the summary. |

**Justification:** Reading supplied feedback and drafting are reversible and stay within the current session. Copying requires a direct user action. Persistent writes and sending could expose customer information or distribute an unreviewed summary, so they’re blocked.

## 06 Verification · AI Testing & Measurement
**Check before the draft is ready:** Verify that:

- The number of comments matches the supplied feedback; don’t treat comments as distinct customers.
- Every theme is supported by one or more supplied comments.
- Unclear, mixed, or uncategorized comments remain visible for review.
- The summary doesn’t add unsupported causes, trends, customer details, or product recommendations.
- The output includes the required sections: positive feedback, negative feedback, key takeaway, and follow-up to consider.

**Failure behavior:** If a check fails, mark the draft **Needs review**, show what could not be verified, and ask the Product Manager to correct or review it. Don’t present it as ready to share.

- **The check before output ships:** _what is checked, against what bar._
- **Failure behaviour:** _what happens when the check fails. Silently dropping the output is never the answer._

## Eval plan

_Stub. Module 5 fills this in: the human eval rubric in `05-evals/human-rubric.md`, with its pass bar._

## Out of scope

_Any capability not in the verb list, and any action tiered `blocked`. Both are decisions on the record, not omissions._
