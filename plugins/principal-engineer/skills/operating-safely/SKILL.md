---
name: operating-safely
description: Use when an action touches live systems, shared state, or things that do not come back - deleting, overwriting, restarting, killing processes, editing shared config, handling secrets, or working beside concurrent sessions. Encodes the destructive-op guards, secrets hygiene, and concurrent-session safety. Use before the risky command, not after, even when the command looks routine.
---

# Operating safely

**REQUIRED BACKGROUND:** the `principal-engineering` skill.

## Overview

Operational damage is asymmetric: the command takes a second, the recovery takes the weekend, and some things (production data, secret exposure, a colleague's uncommitted work) do not come back at all. The rules here are cheap in the moment and exist because each was once expensive.

## Destructive operations

- **Look at the target first.** Before deleting or overwriting, read what is there; before dropping, count what would drop.
- **Targeted over bulk.** Name the specific service, volume, file, or row set; never the flag that takes everything down with it. Bulk teardown commands that include volumes or data are off the table without an explicit, per-instance confirmation.
- **The operator owns live process lifecycles.** Ask before restarting or killing live services and long-running processes; a session that kills what it did not start is operating blind on someone else's state.
- **Ordered operations state the cost of wrong order** before starting (deploy then migrate then clean up; wrong order = silent data loss), and every state-changing procedure knows its rollback or knows plainly that none exists.
- **Database changes:** applied migrations are immutable; destructive statements need explicit confirmation with the row counts on the table.

## Secrets

- Names and structural checks only: verify a secret exists, is non-empty, matches the expected shape. Never read or print the value; never decrypt secrets to disk; never paste one into a log, a test, or a prompt.
- If secret tooling or auth fails or times out: pause and say so. Working around a secrets gate is the one shortcut that is never authorized by urgency.

## Concurrent sessions and shared state

- **Never revert, checkout, overwrite, or commit files you did not change in this session.** Uncommitted changes you did not make belong to someone: surface them and build on top or wait, never clean them up.
- Report residual state at handoff: what is uncommitted, what is merged-but-not-pushed, what is owed, so the next session is not archaeologizing yours.
- One writer per file during parallel work; concurrent writers get their own files or their own worktrees.
- Staging is explicit: name the paths (`git add <paths>`, never the add-everything flag), so a commit cannot capture files you did not mean to ship, including another session's work. One task per commit keeps every change attributable and revertable on its own.

## Shared config and resources

- Config edits are minimal diffs: preserve indentation, quoting, and key order; add no unrequested keys. Config files are shared state with more readers than authors.
- Clean up what you spawn: simulators, containers, worktrees, background processes. Orphaned runtimes accumulate silently until the machine is swapping; when a machine is slow with no process pegging CPU, count the orphans before blaming anything else.

## Common mistakes

- Confirming the operation with yourself. The dangerous ops need the operator's yes, per instance; approval in one context does not extend to the next.
- Pattern-matching a known failure and firing the known remedy (restart it, clear it, reset it) before checking that the evidence supports this specific cause.
- Treating a dry run's success as the live run's safety. The dry run validates shape, not consequence.
- Cleaning a workspace that was not yours to clean.
