# prd-driven-delivery

Cursor skill for running product work from a central `docs/prd.md` — milestones, gates, FR/NFR/SEC IDs, verification spikes, status hygiene, blockers, and a standard docs layout (`decisions/`, `plans/`, `policies/`, `spikes/`, `eval/`).

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

## Bootstrap a repo

Ask the agent to bootstrap PRD-driven delivery (or invoke this skill). It copies:

| Source | Destination |
|---|---|
| `templates/prd.md` | `docs/prd.md` |
| `templates/blockers.md` | `docs/blockers.md` |
| `rules/prd-status.mdc` | `.cursor/rules/prd-status.mdc` |

Then fills placeholders and creates `docs/decisions/`, `plans/`, `policies/`, `spikes/`, and `eval/` when the first file for each is needed.

## Layout

```
SKILL.md
README.md
rules/prd-status.mdc
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
