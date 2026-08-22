---
name: principal-engineering
description: Use when doing any non-trivial engineering work - implementing, debugging, refactoring, configuring, operating, or investigating why a system misbehaves - or any change where being wrong has a cost. Encodes the evidence-over-theory discipline, the hard safety rules, and the pre-change checkpoint. Use whenever code, data, or infrastructure is about to change or must be understood before it can, even if the task looks routine or is only "find out why". Foundation for the sibling skills.
---

# Principal engineering

## Overview

This level rests on checking what the system actually does rather than recalling a pattern for it. **Ground every decision in the real code and data, fail loud, keep one home per fact, and never claim done without the verification that proves it.**

Sibling skills carry the depth: `grounding-before-coding`, `handling-failures`, `keeping-one-source-of-truth`, `verifying-before-done`, `operating-safely`, `scoping-changes`, `testing-changes`, `writing-unit-tests`, `guarding-architecture`, `adding-dependencies`. Load the matching one on top of this.

## When to invoke

| The task is | Also load |
|---|---|
| Starting a change, a debug, or work in unfamiliar code | `grounding-before-coding` |
| Writing or touching any error path, fallback, or default | `handling-failures` |
| Adding data, config, state, or a second copy of anything | `keeping-one-source-of-truth` |
| Claiming "done", "fixed", or "passing" | `verifying-before-done` |
| Deleting, overwriting, restarting, or touching secrets or live systems | `operating-safely` |
| Deciding how big a fix should be, or noticing scope move mid-task | `scoping-changes` |
| Deciding what tests a change owes, or facing an empty test diff | `testing-changes` |
| Writing or fixing a unit test, or taming a flaky or unreadable one | `writing-unit-tests` |
| Crossing module boundaries or touching stated principles | `guarding-architecture` |
| Adding, updating, vetting, or removing a package, library, or base image | `adding-dependencies` |

Writing the documents around the work (specs, decisions, changelogs, runbooks, postmortems, issues) is the technical-writer plugin's job where installed; these skills govern the engineering itself and defer to those for the prose.

## Scope limits

- It is not a style guide: formatting, naming taste, and framework choice belong to the repository's own conventions, which win.
- It does not replace project instructions: CLAUDE.md and repository rules outrank everything here.
- It does not make product decisions: what to build comes from the owner; this governs how built things stay true and safe.

## Mandatory checkpoint before a non-trivial change

Before writing the first line, state in working notes:

`Grounded: <what you read or ran to know the current behavior> | Blast radius: <what this change touches> | Invariants: <what must not break> | Verify: <the command that will prove it worked>`

Fill it from the code and data, not from memory or plausibility. A field you cannot fill is the work you do first.

## Hard rules

Non-negotiable, in every repository:

- **No silent error swallows.** Every catch and failure path logs and rethrows, returns a typed failure the caller must handle, or enters an explicitly documented degraded mode. A new silent swallow is an automatic review BLOCKER. See `handling-failures`.
- **An applied migration is immutable history.** Schema corrections are new additive migrations, never edits to an applied one.
- **Never claim verified without naming what was checked.** A "done" claim names the command and its result, quotes the output of every failing test, and names every skipped step. See `verifying-before-done`.
- **Secret values are never read, printed, or decrypted to disk.** Names and structural checks only; an auth failure means pause, never bypass.
- **Destructive operations need eyes first.** Look at the target before deleting or overwriting; ask before restarting or killing live services; prefer targeted operations over bulk ones.
- **Evidence beats theory.** Profile, query, and read before concluding; a signal that pattern-matches a known failure may have a different cause, so check that the evidence supports the specific action, not the familiar one.

## Risk tiers set the rigor

Not all changes deserve the same rigor. The rules hold at every tier; the tier sets how much proof they demand. **What sits in the top tier is the project's to declare**: money paths in one system, the sales pipeline in another, stored user data, a medical record, a safety gate, an irreversible migration. The project's rules or CLAUDE.md name its top-tier paths; when they do not, ask what the system must never get wrong, and treat the answer as the declaration.

Top-tier work gets maximum rigor: invariant tests, independent verification, and the full checkpoint taken literally. Ordinary paths get standard rigor. Tooling and throwaway work still obey the hard rules (a silent swallow in a script still hides failures) but earn no gold-plating. State the tier when it is not obvious; the expensive mistake is running top-tier work at tooling rigor, and the wasteful one is the reverse.

## The rule lifecycle

When something bites twice, it becomes a written rule with its provenance (what happened, when, how to avoid it); once is learning. A rule that keeps triggering gets sharpened; a rule whose underlying cause is fixed gets retired. Recording the incident behind each rule is what stops rules from being cargo-culted or wrongly deleted later.

## Common mistakes

- Acting on a document's claim about the system instead of the system. Doc status goes stale fast; the code and the history are the record.
- Fixing the symptom that pattern-matched instead of the cause the evidence shows.
- Treating "the tests are green" as "the change works". A green suite over code that cannot work means the suite does not run or does not test.
- Leaving a duplicate untouched in code you are changing. A duplicate in code you touch gets absorbed as part of the work; one you merely noticed elsewhere gets surfaced and tracked, not silently fixed and not silently left. See `keeping-one-source-of-truth`.
- Growing a fix past its trigger because improvements were adjacent. See `scoping-changes`.
