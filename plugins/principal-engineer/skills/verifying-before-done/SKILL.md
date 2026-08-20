---
name: verifying-before-done
description: Use when about to say "done", "fixed", "passing", or "shipped" - or when reporting the outcome of any change. Encodes verification as the definition of done: run the proof, report faithfully, distrust green suites, own failing gates. Use before every completion claim, even when the change was small and obviously correct, which is when this is skipped.
---

# Verifying before done

**REQUIRED BACKGROUND:** the `principal-engineering` skill.

## Overview

Done means verified, and verified names what was checked. Careful work is not a check: the two defects that ship in "obviously fine" changes were both caught, in this corpus's history, by the verification that almost got skipped.

## The discipline

1. **Run the proof, paste the result.** The verify command from the pre-change checkpoint gets executed, and its actual output backs the claim. "Verified" is always "PASS (checked X and Y)", never a bare checkmark.
2. **Report faithfully, both directions.** Tests that fail are reported with their output; steps that were skipped are named as skipped; and work that is done and verified is stated plainly without hedging. Underclaiming verified work wastes the reader's re-verification exactly like overclaiming wastes their trust.
3. **Distrust green.** A green suite over code that cannot work means the suite does not run, does not cover, or cannot fail. When a result seems too clean for the change's size: confirm the test executed (run it alone, watch it appear), and confirm it can fail (break the code, watch it go red, unbreak it). A test that never ran and a gate that never fires are the two failure classes that produce confident wrong "done"s.
4. **Distinguish the tiers.** Implemented (in the repo) is not deployed (live) is not externally verified (checked in the external system). Never claim a later tier from evidence of an earlier one.
5. **Lookback before declaring complete.** Sweep the diff: no unrelated changes, no planning residue in code or comments, docs updated in the same change, every acceptance criterion actually met rather than approximately met.
6. **Independent verification for top-tier changes.** The author of a change is the worst-placed person to verify it; for the project's declared critical paths (money, sales, stored data, safety, whatever the system must never get wrong) and for irreversible migrations, the verifier is someone or something that did not write the code. When no independent verifier is reachable in time, use the nearest substitute and name it as the weaker form it is; downgrading the check silently is the failure, downgrading it visibly is a decision.

## Failing gates you own

Any failing test in a gate you own gets fixed: attribute the origin first, then fix it regardless of whose it is. "Pre-existing" is a footnote in the report, never an excuse in the gate. The one exception is procedural, not evasive: a pre-existing red on the main branch that blocks an unrelated green fix gets surfaced with an offer to merge the green fix anyway, decided by the operator.

## Common mistakes

- Declaring done from the diff looking right. The diff looking right is the hypothesis; the verify command is the experiment.
- Running the whole suite instead of the targeted proof, and reading "no new failures" as "my change works". A suite that never covered the path cannot vouch for it.
- Verifying the happy path of a change whose risk is in the failure path.
- "Tests pass locally" as the terminal claim for a change whose risk is environmental (config, migrations, permissions, prod data shape).
- Fixing the test instead of the code when red is inconvenient. The test was the messenger.
