# Principal engineer

Engineering discipline as skills: the judgment layer for coding agents, distilled from conventions used across my own repositories. Evidence over theory, failures that cannot pass silently, one home per fact, verification as the definition of done, and fixes that never outgrow their trigger.

One core skill holds the hard rules, the risk tiers, and the routing; ten discipline skills build on it.

| Skill | Use when |
|---|---|
| [principal-engineering](plugins/principal-engineer/skills/principal-engineering/SKILL.md) | Any non-trivial engineering work. The foundation: the pre-change checkpoint, the hard rules, the rule lifecycle. |
| [grounding-before-coding](plugins/principal-engineer/skills/grounding-before-coding/SKILL.md) | Starting a change, a debug, or work in unfamiliar code: map the real code and data first, never guess conventions. |
| [handling-failures](plugins/principal-engineer/skills/handling-failures/SKILL.md) | Any error path, catch block, fallback, or default: the no-silent-swallows contract. |
| [keeping-one-source-of-truth](plugins/principal-engineer/skills/keeping-one-source-of-truth/SKILL.md) | Adding data, config, state, or anything that could exist twice: derive rather than store, absorb duplicates. |
| [verifying-before-done](plugins/principal-engineer/skills/verifying-before-done/SKILL.md) | About to say done, fixed, or passing: run the proof, distrust green suites, own failing gates. |
| [operating-safely](plugins/principal-engineer/skills/operating-safely/SKILL.md) | Deleting, overwriting, restarting, secrets, live systems, concurrent sessions. |
| [scoping-changes](plugins/principal-engineer/skills/scoping-changes/SKILL.md) | Sizing a fix, drifting scope, "while you're at it": size to trigger, decompose instead of descoping. |
| [testing-changes](plugins/principal-engineer/skills/testing-changes/SKILL.md) | What tests a change owes: tests move with behavior, the bug-regression pattern, discriminating assertions. |
| [writing-unit-tests](plugins/principal-engineer/skills/writing-unit-tests/SKILL.md) | The craft of the tests themselves: behavior not implementation, names as claims, determinism, mock policy. |
| [guarding-architecture](plugins/principal-engineer/skills/guarding-architecture/SKILL.md) | Structural invariants as named, enforced contracts: statement, rationale, guard; violations mean redesign. |
| [adding-dependencies](plugins/principal-engineer/skills/adding-dependencies/SKILL.md) | Exhaust what you have, vet what you take, pin what you took: the dependency ladder, posture declarations, pin-and-prove updates. |

Pairs with the [technical-writer](https://github.com/riekelt/technical-writer) plugin, which governs the documents around the work (specs, decisions, changelogs, runbooks, postmortems, issues); these skills govern the engineering itself and defer to it for the prose.


## Install

Claude Code:

```
/plugin marketplace add riekelt/principal-engineer
/plugin install principal-engineer@principal-engineer
```

Other agents: point the platform's plugin loader at `plugins/principal-engineer/`, or symlink the directories under `plugins/principal-engineer/skills/` into the agent's skills directory.

## Repository layout

```
.claude-plugin/marketplace.json          # Claude Code marketplace manifest
.agents/plugins/marketplace.json         # generic agents marketplace manifest
.github/workflows/release.yml            # semantic-release on push to main
.releaserc.json                          # release config; stamps versions into the plugin manifests
plugins/principal-engineer/
  .claude-plugin/plugin.json             # per-platform plugin manifests
  .codex-plugin/plugin.json
  .cursor-plugin/plugin.json
  evals/evals.json                       # persisted pressure-test prompts
  skills/
    principal-engineering/               # core: checkpoint, hard rules, routing
    grounding-before-coding/
    handling-failures/
    keeping-one-source-of-truth/
    verifying-before-done/
    operating-safely/
    scoping-changes/
    testing-changes/
    writing-unit-tests/
    guarding-architecture/
    adding-dependencies/
```

## Releases

Conventional Commits on `main` drive semantic-release: commit subjects become the changelog, and the release stamps the version into `package.json` and all three plugin manifests. `CHANGELOG.md` is generated; do not hand-edit it.

## License

MIT
