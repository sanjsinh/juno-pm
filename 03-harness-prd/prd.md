# AI PRD · Juno

> Module 3 · Write an AI PRD Engineers Can Build from. The AI product requirements doc specifying all six harness surfaces, built with the **M3 · AI PRD Builder** (archetype and starting settings from the **M3 · Harness Architecture Decider**). Paste the tool's markdown over this file.

## Problem & user

_The user problem and who has it._

**Problem:** Product Managers need to turn scattered customer feedback into a concise, evidence-based weekly summary for executive review. Doing this manually takes time, and recurring themes or important individual issues can be hard to spot.

**User:** The Product Manager preparing the weekly product review. Executives are the audience for the summary, not the primary user of Juno.____


## The harness

| Surface | Specification |
|---|---|
| 01 Context | Juno reads only the feedback batch and optional period or customer count provided by the PM. If feedback is missing, ask for it. |
| 02 Tools | Juno can read and count comments, group them into candidate themes, draft the review, and check that themes have supporting comments. |
| 03 Loop | Allow up to 3 turns per request. Stop and ask for PM review if input is missing, ambiguous, contradictory, or the draft can’t be grounded… |
| 04 Memory | Keep the input and draft only for the current session; expire them when it ends. Don’t retain them across sessions. |
| 05 Permissions | read auto · draft auto · write confirm · send blocked |
| 06 Verification | Before presenting the draft, check comment counts, evidence for each theme, visibility of mixed or unclassified comments, and that no… |

## 01 Context · Data Requirements

**Required sources, and exclusions**

Juno reads only the feedback batch and optional period or customer count provided by the PM. If feedback is missing, ask for it. Don’t infer customer count or pull in other sources

**How fresh must it be**

Juno should use it **as of the time it’s submitted**. It shouldn’t claim the batch is current beyond that or refresh it automatically.

## 02 Tools · System Capabilities

**The verb list**

Juno can read and count comments, group them into candidate themes, draft the review, and check that themes have supporting comments. It cannot search external systems, save the summary, or send it.

**Deliberate omissions**

- **External web search:** Could add information unrelated to the supplied feedback and make the summary harder to verify.
- **CRM, support, and analytics integrations:** They aren’t needed for the current prototype. Adding them would broaden access and require decisions about data quality, freshness, and permissions.
- **Customer names and contact details:** They aren’t needed to summarize themes and could expose personal information.
- **Saving summaries to company systems:** The PM should review the draft first; persistent writes add risk and aren’t needed for this flow.
- **Sending or publishing summaries:** Juno shouldn’t distribute an unreviewed draft. The PM controls sharing.
- **Product recommendations or root-cause claims:** The feedback sample alone may not support those conclusions. Juno can surface themes and follow-up questions, while the PM decides what to investigate or prioritize.

## 03 Loop · AI Costs & Latency

**Turn ceiling and escalation**

Allow up to 3 turns per request. Stop and ask for PM review if input is missing, ambiguous, contradictory, or the draft can’t be grounded in the feedback

**Latency and cost target**

Answers back in under 8 seconds almost every time, and cost per task should be $0.10

## 04 Memory · Data Requirements

**What persists, at what scope**

Keep the input and draft only for the current session; expire them when it ends. Don’t retain them across sessions.

**Expiry and write rules**

**Expiry:** Supplied feedback, draft summaries, and model-generated themes expire when the session ends. Nothing persists across sessions.

**Correction precedence:** The PM’s explicit correction overrides Juno’s generated theme or summary for the current session. The original feedback remains the source record; if a correction conflicts with what the feedback says, Juno should flag the discrepancy for review rather than silently changing the source.

## 05 Permissions · AI Risks & Mitigations

**The permission tier**

| Side-effect class | Tier |
|---|---|
| read | auto |
| draft | auto |
| write | confirm |
| send | blocked |

**Justification**

| Action | Blast radius | Can the PM undo it? |
|---|---|---|
| **Read supplied feedback** | Limited to the current request and the data the PM pasted. | Nothing changes, so no undo is needed. |
| **Draft a summary** | Limited to the draft shown to the PM; it isn’t shared or saved automatically. | Yes. The PM can edit or discard it. |
| **Copy summary** | The text enters the device clipboard and could be pasted elsewhere by the PM. | Usually: copying again replaces it. Juno can’t undo a paste into another app. |
| **Send or publish** | Could reach recipients outside the PM’s control. | Not reliably; recipients may already have seen or copied it. Keep this blocked. |

## 06 Verification · AI Testing & Measurement

**The check before output ships**

Before presenting the draft, check comment counts, evidence for each theme, visibility of mixed or unclassified comments, and that no unsupported causes or trends were added. If a check fails, mark it Needs review and explain why.

**Failure behaviour**

**Failure behavior:** If a verification check fails, Juno must not mark the summary ready to share or silently omit it. It should show a **Needs review** draft, identify which check failed, and point to the affected comments or count. If the input is missing or unusable, Juno should explain what’s needed and ask the PM to correct it before generating a summary. The PM can then revise the input or review the flagged draft.

## Eval plan

_Stub. Module 6 fills this in: golden set, pass thresholds, and regression cadence for the verification gate above._

## Out of scope

Any capability not in the verb list above, and any action tiered `blocked`. Both are decisions on the record, not omissions.
