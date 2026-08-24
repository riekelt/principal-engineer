---
description: Run the pre-change checkpoint before starting a change
argument-hint: "[task description]"
---

Run the pre-change checkpoint from the `principal-engineering` skill for the task in the arguments (default: the task currently being discussed in this session).

Arguments: `$ARGUMENTS`

Sequence:

1. Load `principal-engineering` and fill its checkpoint from the real repository, never from belief: `Grounded | Blast radius | Invariants | Verify`. Each field carries the evidence behind it: the files read, the callers found, what must not break, the command that will prove the change. State the declared risk tier beside the checkpoint, since it sets the rigor.
2. Route the sibling skills the task calls for, per the core skill's routing table, and name which ones apply.
3. Report the filled checkpoint and the routing to the user before any change is made. An unfillable field is the finding: name what must be grounded first rather than guessing it.
