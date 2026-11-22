---
name: prd-driven-delivery
description: >-
  Authors and runs product work from a central docs/prd.md — drafts a PRD from
  handoffs, meeting notes, chats, and briefs; asks clarifying questions when
  scope is unclear; then bootstraps milestones, gates, FR/NFR/SEC IDs,
  verification spikes, status hygiene, and blockers. Use when writing or
  updating a PRD, turning notes into requirements, starting PRD-driven
  delivery, scaffolding docs/prd.md, updating story Status, opening gates, or
  when the user mentions PRD.md, section 13, M*, G*, V-*, or docs/blockers.md.
---

# PRD-driven delivery

**Global install:** `~/.cursor/skills/prd-driven-delivery/`. Copy templates into a repo; prefer the repo’s `docs/prd.md` once it exists.

`docs/prd.md` is the source of truth for **scope, requirements, and task sequencing**. Do not invent parallel tracks that skip a gate.

## When to use

| User intent | Action |
|---|---|
| “Write a PRD” / notes → requirements | Run **Author a PRD** below |
| New project / “set up PRD-driven delivery” | Author if needed, then **Bootstrap** |
| “What’s next?” / pick work | Read section 13 + gates; pick the next unblocked `P0`/`P1` story |
| Start / finish / block a story | Update `Status` in the same change (see hygiene) |
| Scope change / new provider | Decision record + PRD row first; then code |
| Ticket / epic body | Use section 15 ticket template |

## Author a PRD

Turn source material into a filled [templates/prd.md](templates/prd.md). Do **not** invent binding scope, providers, or gates when the inputs are silent — ask first.

### 1. Gather sources

Collect whatever exists (read in-repo; ask for missing paths or paste):

- Handoffs, architecture briefs, RFCs
- Meeting notes, call transcripts, Slack/email threads
- Prior PRDs, tickets, roadmaps, decision drafts
- Repo baseline (stack, layout, what already ships)

Link durable sources from the PRD header / section 17. Prefer keeping a handoff as its own doc (`docs/handoff.md` or the project’s name) when it owns provider constraints.

### 2. Extract a draft outline (before writing the full PRD)

From the sources, draft only what is supported:

| PRD area | Extract |
|---|---|
| Summary + problem | Who, job-to-be-done, failure modes |
| Goals / non-goals | In-release vs explicitly out |
| Users | Personas and success looks-like |
| Principles | Binding product rules |
| Experience | Primary flow + edge states |
| Requirements | Candidate `FR-*` / `NFR-*` / `SEC-*` |
| Architecture | Must / must-not constraints |
| Milestones + gates | Ordered `M*` / `G*` with DoD |
| Stories + spikes | Section 13 rows + blocking `V-*` |
| Acceptance scenario | Scripted pass/fail demo |

Mark each item **known**, **inferred**, or **unknown**. Inferred items need confirmation; unknown items become questions.

### 3. Clarify when clarity is lacking (required)

If anything material is missing or conflicting, **stop and ask a short series of questions** before writing or rewriting the PRD. Do not pad the PRD with guessed scope.

Use a structured question UI when available (e.g. AskQuestion); otherwise ask conversationally in one batch when possible.

Ask about gaps in this order (skip what sources already settle):

1. **Product & release** — name, one-sentence product, release name, success for v1
2. **Users** — primary persona(s) and MVP success signal
3. **In / out** — must-haves vs explicit non-goals for this release
4. **Constraints** — stack, host, providers that are binding vs flexible
5. **Risks / unknowns** — what must be spiked (`V-*`) before build
6. **Sequencing** — first gate evidence, what “done” means for M0/M1
7. **Security & data** — auth baseline, secrets, retention before external users
8. **Conflicts** — where two sources disagree; which wins until a decision record

Rules for questioning:

- Prefer **few, high-leverage** questions (roughly 3–8 per round). Batch related choices.
- Offer concrete options when the answer space is small; allow free text when it is not.
- After answers, ask a **follow-up round** only for remaining blockers to a coherent section 12–13.
- If the user says “you decide” on a non-binding detail, pick a default, label it inferred in the draft, and continue.
- Never skip questions that would change gates, providers, security baseline, or acceptance scenario.

### 4. Write the PRD

1. Copy [templates/prd.md](templates/prd.md) → `docs/prd.md` (or Bootstrap first if the repo is empty).
2. Fill every section; remove unused subsections only when they truly do not apply.
3. Ensure section 13 stories link requirement IDs and have testable acceptance criteria.
4. Add `V-*` rows for every guessed API/provider integration.
5. Set all new row `Status` to `not started` unless evidence already exists.
6. Create `docs/blockers.md` if missing; leave empty or note open clarification items briefly.
7. For choices that close a conflict, write `docs/decisions/<topic>.md` (Accepted) and link it.

Show the user a brief summary: release goal, milestone list, open spikes, and any remaining unknowns.

### 5. Revise from new notes

When the user adds meeting notes or discussion later: diff against the PRD, propose row-level updates (requirements, stories, non-goals), ask before expanding scope past the current release, then edit `docs/prd.md` and decisions as needed.

## Bootstrap (new or empty repo)

Copy from this skill into the project:

| Source | Destination |
|---|---|
| [templates/prd.md](templates/prd.md) | `docs/prd.md` |
| [templates/blockers.md](templates/blockers.md) | `docs/blockers.md` |
| [rules/prd-status.mdc](rules/prd-status.mdc) | `.cursor/rules/prd-status.mdc` (`alwaysApply: true`) |

If sources exist, run **Author a PRD** to fill placeholders. If not, ask the clarification questions first, then fill.

## Docs layout

This layout is part of PRD-driven delivery (not p-stack). p-stack’s `decisions.tsv` / `.audit/` are agent run trails; product ADRs live under `docs/decisions/`.

Create directories **when the first file is needed**, not as empty scaffolding on day one. Always create `docs/prd.md` and `docs/blockers.md` at bootstrap.

| Path | Role | When to create | Naming |
|---|---|---|---|
| `docs/prd.md` | Scope, requirements, sequencing (source of truth) | Bootstrap | Fixed name |
| `docs/blockers.md` | What is stuck right now; clear with evidence | Bootstrap | Fixed name |
| `docs/handoff.md` (or project-specific brief) | Hosting, SDKs, provider links, non-normative sketches | When a handoff/brief exists | PRD owns scope; handoff owns provider constraints on conflict |
| `docs/decisions/` | Binding architecture/provider/hosting choices after the change rule | First ADR | `<topic>.md` (e.g. `channel-architecture.md`). Status: Proposed / Accepted / Superseded |
| `docs/plans/` | Multi-phase plans that epics point at without stuffing the PRD | First multi-epic plan | `<topic>.md` (e.g. `mews-concierge.md`). Link from section 13 / DoD |
| `docs/policies/` | Retention, consent, data, demo-data rules | First policy story (often pre-external users) | `<topic>.md` (e.g. `audio-and-traces.md`) |
| `docs/spikes/` | Written `V-*` results so implementation does not guess APIs | First verification spike | `<V-ID>.md` matching the PRD spike ID |
| `docs/eval/` | Metric baselines and scenario pass/fail artifacts | First baseline eval story | Date + versions in the artifact; PRD section 11 is the metric list |

Stub shapes: [templates/decision.md](templates/decision.md), [templates/plan.md](templates/plan.md), [templates/policy.md](templates/policy.md), [templates/spike.md](templates/spike.md).

## Document layers

| Layer | ID pattern | Role |
|---|---|---|
| Milestone / epic | `M0`–`Mn` | One epic per milestone. Close only when section 12 DoD is met. |
| Story | `M0.1`, `M1.3`, … | One ticket per story. Copy acceptance criteria verbatim. |
| Requirement | `FR-*`, `NFR-*`, `SEC-*` | Link every story to requirement IDs. Done only when a done story demonstrates them. |
| Gate | `G0`–`Gn` | Hard stop. Next epic does not start until passed or explicitly waived. |
| Verification | `V-*` | Time-boxed spike/smoke that can **block** a story. Write the result before implementation continues. |

Work items come from **section 13** only (stories, gates, `V-*`). Prefer those IDs over ad-hoc task lists.

## Status hygiene

Valid values only: `not started` | `in progress` | `blocked` | `done`.

| Event | Set Status to | Also |
|---|---|---|
| Begin a story, requirement, or `V-*` | `in progress` | Confirm `Depends on` / gates |
| Acceptance criteria met with **evidence** | `done` | Mark linked `FR-*` / `NFR-*` / `SEC-*` `done` only when a done story demonstrates them |
| Spike, missing credential, or provider limit | `blocked` | Name the blocker in `docs/blockers.md` |
| New story/requirement needed | Add the row first | Do not invent parallel tracks that skip a gate |

Rules:

- Update `Status` in the **same change** that starts, blocks, or finishes work (or in the same turn before coding if still `not started`).
- Merged or written code with no evidence stays `in progress`, not `done`.
- When blocked, update both the PRD row and `docs/blockers.md` in the same turn.
- Do **not** ask the user whether to update the PRD for these transitions; do it and mention the ID briefly in the reply.
- Confirm the always-apply rule exists at `.cursor/rules/prd-status.mdc` (from [rules/prd-status.mdc](rules/prd-status.mdc)).

## Sequencing

1. Implement **one milestone at a time**, in order.
2. Stories inside a milestone may proceed in parallel only when `Depends on` is satisfied.
3. Gates are hard stops — do not start the next epic until the gate has evidence or a written waiver.
4. `V-*` spikes that block a story must finish (or record a mismatch) before that story’s implementation.
5. Security controls ship with the surface that introduces them — do not defer auth/secrets to a late “hardening” epic unless the PRD explicitly says so.

## Change control

- New feature, provider, or hosting change → short decision record in `docs/decisions/` **and** update the affected requirement/story **before** work starts.
- Do not silently substitute stacks, hosts, or APIs called out as binding in the PRD architecture section.
- Eval/regression rule (when the PRD defines one): after baseline eval stories exist, changes that touch covered paths re-run those scenarios and record deltas.

## Ticket body (from PRD section 15)

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

A story is `done` only when acceptance criteria have evidence (test name, command output, recording note, or eval artifact). “Code merged” is not done.

## Priority labels (section 13)

| Pri | Meaning |
|---|---|
| `P0` | Blocks the current gate |
| `P1` | Required for the named release |
| `P2` | Required before external users / broader rollout |
| `P3` | Not on the critical path |

## Agent workflow checklist

**Authoring**

```
- [ ] Gather handoff / notes / chats / repo baseline
- [ ] Outline known vs inferred vs unknown
- [ ] Ask clarification questions for material gaps (do not guess gates/providers/security)
- [ ] Write docs/prd.md from template; link decisions; summarize for the user
```

**Executing** (existing PRD)

```
- [ ] Read docs/prd.md sections 0, 12, 13 (and 17 map if present)
- [ ] Check docs/blockers.md for open stops
- [ ] Confirm Depends on / gate for the chosen story
- [ ] Set Status → in progress (same turn)
- [ ] Implement; gather evidence for acceptance criteria
- [ ] Set Status → done (or blocked + blockers.md)
- [ ] Mention story ID briefly in the reply
```

## Templates and rules

- Full PRD skeleton: [templates/prd.md](templates/prd.md)
- Blockers stub: [templates/blockers.md](templates/blockers.md)
- Decision / plan / policy / spike stubs: [templates/decision.md](templates/decision.md), [templates/plan.md](templates/plan.md), [templates/policy.md](templates/policy.md), [templates/spike.md](templates/spike.md)
- Always-apply Cursor rule: [rules/prd-status.mdc](rules/prd-status.mdc)
- Filled example (voice-agent product): see a repo that already uses this pattern’s `docs/prd.md` — structure only; do not copy product-specific requirements into a new project.
