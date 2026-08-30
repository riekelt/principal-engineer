---
name: verifying-before-done
description: "Use when about to say \"done\", \"fixed\", \"passing\", or \"shipped\" - or when reporting the outcome of any change. Encodes verification as the definition of done: drive the change at its surface, run the verify command, report faithfully, distrust green suites, own failing gates. Use before every completion claim, even when the change was small and obviously correct, which is when this is skipped."
---

# Verifying before done

**REQUIRED BACKGROUND:** the `principal-engineering` skill.

## Overview

Done means verified, and verified names what was checked. Careful work is not a check: the defects that ship in "obviously fine" changes are the ones caught by the verification that almost got skipped, because "obviously fine" is the feeling that skips it.

## The discipline

1. **Drive the change at its surface, capture what it did.** Run the smallest path that executes the changed code in the running system; "Verify at the surface" below expands this.
2. **Run the verify command, paste its output.** The verify command comes from the pre-change checkpoint; it proves the gates hold, and its actual output backs that part of the claim. Redact secret values from output before quoting it; a report travels further than a terminal (`operating-safely` owns secrets hygiene). "Verified" is always "PASS (checked X and Y)", never a bare checkmark.
3. **Report faithfully, both directions.** Report tests that fail with their output, name skipped steps as skipped, and state verified work plainly without hedging. Underclaiming verified work wastes the reader's re-verification exactly like overclaiming wastes their trust.
4. **Distrust green.** A green suite over code that cannot work means the suite does not run, does not cover, or cannot fail. When a result seems too clean for the change's size, confirm the test executed (run it alone, watch it appear), and confirm it can fail (break the code, watch it go red, unbreak it). A test that never ran and a gate that never fires produce confident wrong "done"s.
5. **Distinguish the tiers.** Implemented (in the repo) is not deployed (live) is not externally verified (checked in the external system). Never claim a later tier from evidence of an earlier one.
6. **Lookback before declaring complete.** Sweep the diff: no unrelated changes, no planning residue in code or comments, docs updated in the same change, every acceptance criterion actually met rather than approximately met.
7. **Independent verification for top-tier changes.** The author of a change is the worst-placed person to verify it. For the project's declared critical paths (money, sales, stored data, safety, whatever the system must never get wrong) and for irreversible migrations, the verifier is someone or something that did not write the code. When no independent verifier is reachable in time, use the nearest substitute and name it as the weaker form it is; downgrading the check silently is the failure, downgrading it visibly is a decision.

## Verify at the surface

The gates (the verify command, the suite, the build) prove the repository holds; the change is proven where its caller meets it. That surface is the command line that runs it, the endpoint that serves it, the screen that renders it, or the consumer that imports it.

- **Drive the smallest path that executes the changed code, in the running system.** Run a changed flag with the flag set, send a changed handler its request, trigger the error of a changed error path. An internal function is not a surface: something calls it, and that caller ends at a surface, so observe there.
- **Read tests as the author's evidence, not the verification.** A test says what to drive; the gates re-run it.
- **Probe beside the change.** The happy path confirms the claim; the neighbors test it: the empty value, the repeated call, the conflicting option, the adjacent error the change did not touch. One probe past the claim is the minimum, and a probe that holds is still reported, because it says what was covered.
- **Quote what the system produced.** The response body, the terminal output, and the rendered screen back the claim. Redact before quoting: tokens, connection strings, and auth headers in a capture leak to every reader of the report. Treat ambiguous output as a failure with the redacted raw capture attached, never interpret it into a pass.
- **Say "no runtime surface" when none exists.** Docs, comments, and type declarations that produce no behavior get that as the verification, never a substitute gate run to fill the space.
- **Never drive destructive paths live.** Where the changed code deletes or writes beyond the workspace and no safe target exists, verify around it and name the unexercised path (`operating-safely` owns the guards).

## Failing gates you own

Attribute the origin first, then fix the test regardless of whose it is. "Pre-existing" is a footnote in the report, never an excuse in the gate. The one exception is procedural: a pre-existing red on the main branch that blocks an unrelated green fix gets surfaced with an offer to merge the green fix anyway, decided by the operator.

## Common mistakes

- Declaring done from the diff looking right. The diff looking right is the hypothesis; the run at the surface is the experiment.
- Running the whole suite instead of the targeted verify command, and reading "no new failures" as "my change works". A suite that never covered the path cannot vouch for it.
- Re-running the gates and calling it verification. Green gates plus an undriven surface is the claim "CI works", not the claim "the change works".
- Verifying the happy path of a change whose risk is in the failure path.
- "Tests pass locally" as the terminal claim for a change whose risk is environmental (config, migrations, permissions, prod data shape).
- Fixing the test instead of the code when red is inconvenient. The test was the messenger.
