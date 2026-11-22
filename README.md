# prd-driven-delivery

Cursor skill for **authoring and running** product work from a central `docs/prd.md` — one source of truth for scope, requirements, and sequencing.

- Draft a PRD from handoffs, meeting notes, chats, and briefs
- Ask clarifying questions when scope, constraints, or gates are unclear (no guessing)
- Bootstrap milestones, gates, FR/NFR/SEC IDs, verification spikes, status hygiene, and blockers
- Keep a standard docs layout (`decisions/`, `plans/`, `policies/`, `spikes/`, `eval/`)

Repo: https://github.com/jpizzle34/prd-driven-delivery

## Install

Global (all projects):

```bash
pnpm dlx skills add jpizzle34/prd-driven-delivery -g
```

Project-only:

```bash
pnpm dlx skills add jpizzle34/prd-driven-delivery -p
```

Or clone into your Cursor skills directory:

```bash
git clone https://github.com/jpizzle34/prd-driven-delivery.git ~/.cursor/skills/prd-driven-delivery
```

Update later with `pnpm dlx skills update prd-driven-delivery -g` (or pull if you cloned).

## When to use

| You want to… | The skill… |
|---|---|
| Write a PRD from notes / a handoff | Runs **Author** |
| Set up PRD-driven delivery in a repo | Bootstraps files, then authors if needed |
| Know what’s next / ship a story | Reads section 13, updates Status, implements with evidence |
| Change provider / host / scope | Decision record + PRD row **before** code |

## Process

Two phases: **author** the PRD, then **execute** against it.

### Phase A — Author (notes → PRD)

1. **Gather sources** — Handoffs, meeting notes, chats, RFCs, tickets, and the current repo baseline.
2. **Outline** — Map sources into PRD areas (problem, goals/non-goals, users, requirements, architecture, milestones/gates, stories/spikes, acceptance scenario). Tag each item *known* / *inferred* / *unknown*.
3. **Clarify gaps** — Stop and ask ~3–8 questions per round for anything material that’s missing or conflicting. Skip what’s already settled. Don’t guess gates, providers, or security.
4. **Bootstrap the repo** (if needed) — Copy `docs/prd.md`, `docs/blockers.md`, and `.cursor/rules/prd-status.mdc` from the skill templates.
5. **Write the PRD** — Fill the template: requirements with IDs, ordered milestones and gates, section 13 stories with acceptance criteria, `V-*` spikes for unknown APIs. Status starts as `not started`. Close conflicts with `docs/decisions/<topic>.md`.
6. **Summarize** — Show release goal, milestone list, open spikes, remaining unknowns.
7. **Revise later** — When new notes arrive, diff against the PRD, propose row updates, ask before expanding release scope, then edit.

Other docs folders (`plans/`, `policies/`, `spikes/`, `eval/`) appear only when the first real file is needed.

### Phase B — Execute (PRD → shipped work)

8. **Pick work** — Read sections 0 / 12 / 13 and `docs/blockers.md`. Choose the next unblocked `P0`/`P1` story from section 13 only. One milestone at a time; respect `Depends on` and gates.
9. **Spike first if blocked** — Run any blocking `V-*`, write `docs/spikes/<V-ID>.md`, then continue (or stop on mismatch).
10. **Start the story** — Set Status → `in progress` in the same turn (no asking). Confirm gate/deps.
11. **Implement** — Build to the acceptance criteria. Security ships with the surface that needs it.
12. **Finish or block**
    - **Done:** evidence exists → Status `done`; mark linked `FR-*` / `NFR-*` / `SEC-*` only when demonstrated.
    - **Blocked:** Status `blocked` + row in `docs/blockers.md`.
    - Code without evidence stays `in progress`.
13. **Pass the gate** — Attach evidence (or a written waiver) before starting the next epic.
14. **Change control** — New feature/provider/host → decision record + PRD row update **before** coding. After eval baselines exist, re-run affected scenarios when those paths change.
15. **Repeat** from step 8 until release gates have evidence.

Throughout execution, the always-on rule keeps Status current; the agent mentions the story ID in the reply and does not invent parallel tracks that skip a gate.

## Clarifying questions

Asked only for gaps, in this order:

1. **Product & release** — name, one-line product, release name, v1 success
2. **Users** — personas and MVP success signal
3. **In / out** — must-haves vs explicit non-goals
4. **Constraints** — binding vs flexible stack / host / providers
5. **Risks / unknowns** — what must be spiked (`V-*`) before build
6. **Sequencing** — first gate evidence; early milestone “done”
7. **Security & data** — auth, secrets, retention before external users
8. **Conflicts** — which source wins until a decision record

## Docs layout

| Path | Role |
|---|---|
| `docs/prd.md` | Scope, requirements, sequencing (source of truth) |
| `docs/blockers.md` | What is stuck right now |
| `docs/handoff.md` (optional) | Provider/SDK constraints; PRD wins on product scope |
| `docs/decisions/` | Binding ADRs after a change rule fires |
| `docs/plans/` | Multi-phase plans epics point at |
| `docs/policies/` | Retention, consent, demo-data rules |
| `docs/spikes/` | Written `V-*` results |
| `docs/eval/` | Metric baselines and scenario artifacts |

## Bootstrap files

| Source in this skill | Destination in the project |
|---|---|
| `templates/prd.md` | `docs/prd.md` |
| `templates/blockers.md` | `docs/blockers.md` |
| `rules/prd-status.mdc` | `.cursor/rules/prd-status.mdc` (`alwaysApply`) |

Stub templates also ship for `decision`, `plan`, `policy`, and `spike`.

## Package layout

```
SKILL.md                 # Agent instructions
README.md
LICENSE
rules/prd-status.mdc     # Always-apply Status hygiene
templates/
  prd.md
  blockers.md
  decision.md
  plan.md
  policy.md
  spike.md
```

## License

MIT
