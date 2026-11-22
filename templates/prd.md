# Product Requirements Document — {{PRODUCT_NAME}}

**Status:** Draft for execution  
**Source:** {{HANDOFF_OR_BRIEF_LINK}} (architecture and integration constraints). This PRD is the source of truth for **scope, requirements, and task sequencing**. The handoff/brief remains the source of truth for **hosting choices, SDK assumptions, and reference links**. If they conflict on product scope, this PRD wins. If they conflict on a provider or runtime constraint, the handoff wins until an explicit decision record changes it.  
**Prepared:** {{YYYY-MM-DD}}  
**Product:** {{ONE_SENTENCE_PRODUCT}}  
**Release name:** {{RELEASE_NAME}}

## 0. How this document drives task management

Create work items from **section 13** only. Do not invent parallel tracks that skip a gate.

| Layer | ID | Use |
|---|---|---|
| Milestone / epic | `M0`–`Mn` | One epic per milestone. Close the epic only when its definition of done in section 12 is met. |
| Story | `M0.1`, `M1.3`, … | One ticket per story. Copy acceptance criteria verbatim into the ticket. |
| Requirement | `FR-*`, `NFR-*`, `SEC-*` | Link every story to the requirement IDs in its row. A story is done only when those requirements are demonstrable. |
| Gate | `G0`–`Gn` | Hard stop. The next epic does not start until the gate is passed or an explicit waiver is recorded. |
| Verification | `V-*` | Spike or smoke test that can **block** a story. Record the result in the ticket (and preferably `docs/spikes/<ID>.md`) before implementation continues. |

**Status column.** Requirement tables, verification spikes, and section 13 story tables use one `Status` value.

| Value | When you set it |
|---|---|
| `not started` | No work has started on the row. |
| `in progress` | Work has started, and the acceptance criteria are not met. |
| `blocked` | A spike, missing credential, or provider limit stops the row. Name that blocker in `docs/blockers.md`. |
| `done` | The story acceptance criteria have evidence. A requirement is `done` only when a `done` story demonstrates it. |

Update `Status` in the same change that starts, blocks, or finishes the row. Merged code with no evidence stays `in progress`.

**Sequencing rule:** Implement one milestone at a time, in order. Stories inside a milestone may proceed in parallel only when their `Depends on` column is satisfied.

**Security rule:** Controls ship with the surface that introduces them. Do not defer authentication and server-side secrets to a late hardening epic unless this PRD explicitly says so.

**Change rule:** A new feature, provider, or hosting change requires a short decision record in `docs/decisions/` and an update to the affected requirement and story before work starts. Do not silently substitute stacks or hosts named as binding in section 10.

**Eval rule (optional):** After baseline eval stories exist, a story that changes covered paths must extend or re-run the affected scenarios and record section 11 deltas against the baseline. Deterministic fixture checks beat subjective scores as sole gates.

**Baseline at PRD time ({{YYYY-MM-DD}}):** {{REPO_BASELINE — stack, layout, what exists vs what does not. Name the first story that changes layout if needed.}}

---

## 1. Summary

{{2–4 paragraphs: what you are building, for whom, what the first release includes, and what waits until later.}}

## 2. Problem

{{Why this product exists. Name the failure modes you are designing against.}}

## 3. Goals and non-goals

### 3.1 Goals ({{RELEASE_NAME}})

1. {{Goal}}
2. {{Goal}}
3. {{Goal}}

### 3.2 Non-goals (not in {{RELEASE_NAME}})

- {{Explicit out-of-scope item}}
- {{Explicit out-of-scope item}}

## 4. Users and jobs

| User | Job to be done | MVP success looks like |
|---|---|---|
| **{{Persona}}** | {{Job}} | {{Observable success}} |
| **Developer** | Run and change the deployables locally and in managed hosts | Documented commands; secrets stay server-side |
| **Operator / evaluator** | See what the system did and why | Can correlate IDs across UI, logs, and traces |

## 5. Product principles

1. {{Principle — short and binding}}
2. {{Principle}}
3. {{Principle}}

## 6. Experience requirements

### 6.1 Primary flow

1. {{Step}}
2. {{Step}}
3. {{Step}}

### 6.2 {{Surface}} UX

- {{Rule}}

### 6.3 Empty, error, and edge states

| State | Required behavior |
|---|---|
| {{State}} | {{Behavior}} |

## 7. Functional requirements

### 7.1 Platform and repository

| ID | Status | Requirement |
| --- | --- | --- |
| `FR-PLAT-01` | not started | {{Requirement}} |

### 7.2 {{Domain area}}

| ID | Status | Requirement |
| --- | --- | --- |
| `FR-…-01` | not started | {{Requirement}} |

Add subsections as needed (`7.3`, `7.4`, …). Keep IDs stable once referenced by stories.

## 8. Non-functional requirements

| ID | Status | Requirement | Target for {{RELEASE_NAME}} |
| --- | --- | --- | --- |
| `NFR-PERF-01` | not started | {{Requirement}} | {{Target or “baseline in M*”}} |
| `NFR-REL-01` | not started | {{Requirement}} | {{Target}} |

## 9. Security and governance requirements

| ID | Status | Requirement |
| --- | --- | --- |
| `SEC-01` | not started | Credentials exist only in server or worker environments. |
| `SEC-02` | not started | Privileged routes authorize the current user. |

## 10. Architecture constraints

These are binding for all stories.

```mermaid
flowchart TD
  Client[{{Client}}]
  API[{{API / BFF}}]
  Worker[{{Worker or service}}]
  Backend[{{Primary backend}}]

  Client -->|HTTPS| API
  Client <-->|{{media or WS if any}}| Worker
  API --> Backend
  Worker --> Backend
```

| Component | Must | Must not |
|---|---|---|
| {{Component}} | {{Must}} | {{Must not}} |

## 11. Metrics

Measured on the real deployment, versions, and demo scenario.

| Metric | Why it is in the PRD |
|---|---|
| {{Metric}} | {{Why}} |

## 12. Milestones, gates, and definitions of done

### Gate G0 — before M1

**Status:** not started

{{Pass condition and required evidence.}}

### M0 — {{Milestone name}}

**Outcome:** {{One sentence}}

**Definition of done:** {{Observable DoD}}

### Gate G1 — before M2

{{Pass condition.}}

### M1 — {{Milestone name}}

**Outcome:** {{One sentence}}

**Definition of done:** {{Observable DoD}}

<!-- Repeat Gate Gn / Mn as needed. Post-release epics are fine; label them clearly. -->

## 13. Work breakdown

Priority: **P0** blocks the current gate, **P1** required for {{RELEASE_NAME}}, **P2** required before external users, **P3** not on the critical path.

### Epic M0 — {{Name}}

| ID | Status | Title | Depends on | Pri | Requirements | Acceptance criteria |
| --- | --- | --- | --- | --- | --- | --- |
| `M0.1` | not started | {{Title}} | — | P0 | `FR-…` | {{Verbatim, testable criteria}} |
| `M0.2` | not started | {{Title}} | `M0.1` | P0 | `FR-…` | {{Criteria}} |

### Epic M1 — {{Name}}

| ID | Status | Title | Depends on | Pri | Requirements | Acceptance criteria |
| --- | --- | --- | --- | --- | --- | --- |
| `M1.1` | not started | {{Title}} | `G0`, `V-…` | P0 | `FR-…`, `SEC-…` | {{Criteria}} |

### Verification spikes (tickets, time-boxed)

These are tasks. They produce a written result. Implementation stories that depend on them do not start on guessed APIs.

| ID | Status | Question | Blocks | Pass looks like |
| --- | --- | --- | --- | --- |
| `V-…-01` | not started | {{Question}} | `M….` | {{Exact evidence: versions, commands, IDs, or a blocking mismatch report}} |

## 14. {{RELEASE_NAME}} acceptance scenario

{{Scripted demo or fixture the team can run end-to-end.}}

**Pass**

- {{Observable pass condition}}

**Fail**

- {{Observable fail condition}}

## 15. Ticket template

Use this body for every story in section 13:

```text
ID:
Status:
Title:
Epic:
Priority:
Depends on:
Requirements:

Scope:
- 

Out of scope:
- 

Acceptance criteria:
- 

Evidence:
- 

SDK / provider notes:
- 
```

A story moves to done when acceptance criteria are checked with evidence (test name, command output, or eval note). “Code merged” is not done.

## 16. Risks and decision rules

| Risk | Task-management rule |
|---|---|
| Installed APIs differ from the handoff sketch | Stop on the spike (`V-*`). Update story notes. Do not swap providers inside the same story. |
| A gate’s capability is unavailable | Gate stays open. Do not fake the capability and call the milestone done. |
| Scope creep into non-goals | Point at section 3.2. Add a decision record and PRD rows before work. |
| Auth is tempting to skip locally | Keep the early auth story P0. Later routes reuse that gate. |

## 17. Documentation map

Create subdirectories when the first file is needed, not empty ahead of time.

| Need | Document |
|---|---|
| Why the system is shaped this way, provider links | {{HANDOFF_OR_BRIEF_LINK}} |
| What to build and in what order | `docs/prd.md` (this file) |
| What is stuck | `docs/blockers.md` |
| Provider, host, or architecture change (ADR) | `docs/decisions/<topic>.md` |
| Multi-phase plan referenced by epics | `docs/plans/<topic>.md` |
| Retention, consent, demo-data, or similar rules | `docs/policies/<topic>.md` |
| Verification spike results (`V-*`) | `docs/spikes/<ID>.md` |
| Metrics and scenario pass/fail artifacts | `docs/eval/` |
| How to run and deploy | `README.md` |

## 18. Suggested execution order

The first tickets to open, in order, are:

1. {{First `V-*` and/or `M0.1`}}
2. {{Next}}
3. {{… → stop for G0}}
4. {{Continue through gates}}

{{RELEASE_NAME}} is complete when gates **G0 through G{{n}}** have evidence attached to their epic tickets.
