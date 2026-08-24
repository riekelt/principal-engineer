---
description: Independently verify a completion claim before it ships
argument-hint: "[claim or scope]"
---

Verify the completion claim named in the arguments per the `verifying-before-done` skill. Default: the most recent done-claim in this session; absent one, the current working-tree change.

Arguments: `$ARGUMENTS`

Sequence:

1. State the claim being verified, in one line, before any check runs.
2. Dispatch a `change-verifier` agent with that claim, the surface where the change meets its caller, and the verify commands. Resolve the commands in this order: the pre-change checkpoint where one was recorded, then the project's own instructions (CLAUDE.md or equivalent), then the repo's declared scripts and CI gates. The verifier is independent: it gets the claim and the code, never the author's reasoning about why the change is correct.
3. Check the report before relaying: a report whose evidence is gates output with no surface driven is incomplete. Re-dispatch the same agent once with the gap named; a second gates-only report goes to the user as UNVERIFIABLE with that reason.
4. Relay its verdicts unaltered: VERIFIED with the surface driven and what was checked, CONTRADICTED with the evidence, or UNVERIFIABLE with what is missing.
5. A CONTRADICTED or UNVERIFIABLE verdict blocks the done-claim: report it as not done, with the verifier's findings as the work remaining. Never soften a verdict in the relay.
