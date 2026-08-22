---
name: grounding-before-coding
description: "Use when starting any non-trivial change, investigating a bug, or working in unfamiliar code - before the first line is written. Also use for pure investigation with no change planned yet - \"dig into this\", \"figure out why\", \"sometimes the export is empty\", intermittent errors after a deploy. Encodes the ground-first discipline: map the real code and data, quote evidence, never guess conventions. Use whenever a change or a conclusion is about to be built from belief instead of from the tree, even under time pressure."
---

# Grounding before coding

**REQUIRED BACKGROUND:** the `principal-engineering` skill.

## Overview

Before writing a spec, a fix, or a first line: map the real code and data. Quote `file:line` and run the query behind every number you rely on. The cost of grounding is minutes; the cost of building on a wrong belief is the whole change plus the incident it causes.

## The discipline

1. **Read the implementations, not the names.** A method called `validate` that does not validate is common enough to be the default assumption. Verify what a thing does before building on what it is called.
2. **Quote your evidence.** Every load-bearing claim in your plan gets a `file:line`, an exact query result, or a command output. When you cannot back a claim, say so out loud instead of assuming it.
3. **Never guess conventions.** How this repo names things, wires dependencies, handles errors, or runs tests is discoverable in minutes. Guessing conventions is how changes arrive that are correct in isolation and wrong in the codebase.
4. **Trust code, not status.** A document's or ticket's self-reported state is not evidence of execution state; adjudicate with the code and the history (`git log -S <symbol>`, grep the tree) before building on it.
5. **Reproduce before fixing.** For bugs: see the failure happen before changing anything. A fix for an unreproduced bug is a guess wearing a diff.
6. **Fix where the callers converge.** A bug report names one symptom on one path; before editing, find every route into the code you are about to touch. When the defect lives in something shared, the guard belongs in the shared place: it is the smaller diff AND the fix that covers the sibling paths the ticket never mentioned. Patching only the reported path repairs the report, not the bug.
7. **Map the invariants a change must not break.** The output of grounding is a map: the touchpoints, the current behavior (quoted), and those invariants. Tests named after old bugs, guards with explanatory comments, and constants encoding hard-won thresholds are the scars that mark earlier incidents.

## Limits of grounding

- Not reading everything: map what the change touches plus one ring around it, at the depth the risk demands.
- Not a substitute for asking: when the code cannot answer an intent question (why is this threshold 7?), the history or the owner can. An unanswerable question becomes a named assumption, never a silent one.
- Not re-grounding what this session already established: ground once, cite it after.

## Common mistakes

- Theorizing from the framework's documentation about what the project's code does. The project forked, wrapped, or misused the framework; the tree tells you which.
- Grounding the happy path only. The invariants live in the error paths and the edge-case guards.
- Trusting a prior session's summary of the code over the code. Open the files the summary names before building on it.
- Skipping grounding because the task "looks like" a previous one. The signal that pattern-matches a known case may have a different cause; check that the evidence supports this case.
