# kaostack

A personal library of [Claude Code](https://claude.com/claude-code) / Cursor skills: markdown `SKILL.md` definitions the agent loads to change how it works on a task, plus supporting playbooks, reference docs, and a small toolkit of scripts.

## Layout

```
skills/
  poteto-mode/            the main agent-style skill, plus its playbooks, references, and scripts
  principle-*/            standalone principle skills poteto-mode and others cite
  architect/ arena/ how/ why/ automate-me/ show-me-your-work/ unslop/
```

Each skill directory has a `SKILL.md` with YAML frontmatter (`name`, `description`, trigger hints) followed by the instructions the agent follows once the skill is invoked. Skills may also ship `references/` (longer docs pulled in on demand) and `scripts/` (executable helpers).

## Skills

| Skill | Purpose |
| --- | --- |
| `poteto-mode` | Agent operating style: concise responses, deliberate subagent use, unslopped prose, simple code, verified work. Routes to the playbooks and principles below. |
| `architect` | Sketch types, signatures, and module structure before writing code; stays in the loop while implementation fills in. |
| `arena` | Spawn N parallel candidates at the same task, pick a base, graft the strongest parts of the losers into it. |
| `how` | Explain how a subsystem works: architecture, runtime flow, onboarding mental models. |
| `why` | Explain why something works the way it does: design rationale, regressions, postmortems, cited from source control, issue trackers, chat, and observability tools. |
| `automate-me` | Mines agent-transcript history plus direct questions to author a personalized "-mode" skill capturing a user's working conventions. |
| `show-me-your-work` | Keeps a reviewable decision-trail log (TSV, one row per decision) for long-running or unattended work. |
| `unslop` | Cuts AI writing tells from any prose surface. |
| `principle-*` | Standalone principles (fix root causes, prove it works, redesign from first principles, separate before serializing shared state, encode lessons in structure) cited by name from `poteto-mode` and other skills. |

### poteto-mode

The largest skill in the repo. `skills/poteto-mode/SKILL.md` defines the operating style and routes to:

- **`playbooks/`** — one file per recurring workflow: bug fixes, features, refactors, perf issues, investigations, PR shipping, babysitting PR review bots, worktree cleanup, session pickup, and more.
- **`references/`** — supporting docs pulled in by playbooks, e.g. `bugbot-triage.md` for classifying automated review comments as fix / dismiss / ask.
- **`scripts/`** — a small Bun/TypeScript toolkit:
  - `orch/` — a plain-file orchestration store (units, verification ledger, inbox, decision gates, Graphite stack frontier) with an `orch` CLI (`orch.ts`) backed by `store.ts`.
  - `watch-pr/` — a PR-watching CLI (GitHub polling, triage policy, rendering).
  - `check-plan.mjs` — lints multi-phase plan documents against the plan template's required structure.
  - `worktree-audit.sh` — read-only audit of git worktrees (size, merge state, dirty state, PR status, last-touched) to inform manual pruning.
  - `bootstrap.ts` — installs the toolkit's own dependencies on first run.

## Working with the scripts

```bash
cd skills/poteto-mode/scripts
bun install
bun test orch watch-pr   # run the test suites
bun run typecheck         # tsc --noEmit --strict over watch-pr
```

## License

MIT, see [LICENSE](LICENSE).
