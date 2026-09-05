# The `.phronesis/` working-directory convention

**Status:** v1.0 — 2026-09-04 · Ledger §15.10.602 (§1-R register row 6; origin `CCO_DISPATCH_CRAWL_POLICY_20260701.md` §3 Lane B)
**Scope:** every skill in this repository. This document is the single home for the convention; each skill's `SKILL.md` carries a short pointer section.

## What it is

A per-task assurance trail. When a Phronesis skill guides a real decision task, the
calling agent keeps a working directory in its own workspace:

```
.phronesis/<task-slug>/
```

and maintains six files in it as the task progresses — so the decision, the evidence,
the exact requests, the receipts, and the follow-up are inspectable afterward without
reconstructing them from chat history.

## The six files

| # | File | Written | Contents |
|---|------|---------|----------|
| 1 | `decision_requirement.md` | first, before any call | the decision in one sentence, decision owner, materiality, resolution horizon |
| 2 | `evidence_notes.md` | as evidence is gathered | evidence consulted, with sources and timestamps |
| 3 | `action_boundary_request.json` | at call time | the exact action-boundary / assessment request sent, verbatim |
| 4 | `decision_asset.json` | on response | the receipt / forecast output returned, verbatim — never edited |
| 5 | `outcome_followup.md` | at decision time | what was decided, and the trigger that revisits it |
| 6 | `review_log.md` | append-only | dated review notes over the task's life; entries are appended, never rewritten |

## Rules

- **Verbatim receipts.** Files 3 and 4 are byte-faithful copies of what was sent and
  received. Summaries and interpretation belong in files 2, 5, and 6.
- **Append-only review.** `review_log.md` grows; it is never rewritten.
- **One directory per task.** A new decision task gets a new `<task-slug>`.
- **The trail is the agent's.** It lives in the calling agent's workspace, not in this
  repository and not on the Phronesis platform. Nothing in it is transmitted anywhere
  by this convention.

## Honest-claim note

This convention is an **instruction the skills carry from v1.0 forward**, not a claim
about behavior before it existed. A skill's `SKILL.md` section states what the agent
following it is instructed to do; it asserts nothing else.
