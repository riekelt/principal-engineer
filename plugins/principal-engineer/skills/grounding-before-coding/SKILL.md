---
name: grounding-before-coding
description: "Use when starting any non-trivial change, investigating a bug, or working in unfamiliar code - before the first line is written. Also use for pure investigation with no change planned yet - \"dig into this\", \"figure out why\", \"sometimes the export is empty\", intermittent errors after a deploy. Encodes the ground-first discipline: map the real code and data, quote evidence, never guess conventions. Use whenever a change or a conclusion is about to be built from belief instead of from the tree, even under time pressure."
---

# Grounding before coding

**REQUIRED BACKGROUND:** the `principal-engineering` skill.

## Overview

Before writing a spec, a fix, or a first line: map the real code and data. Quote `file:line`. Run the query. The cost of grounding is minutes; the cost of building on a wrong belief is the whole change plus the incident it causes.

## The discipline

1. **Read the implementations, not the names.** A method called `validate` that does not validate is common enough to be the default assumption. Verify what a thing does before building on what it is called.
2. **Quote your evidence.** Every load-bearing claim in your plan gets a `file:line`, an exact query result, or a command output. A claim you cannot back gets said out loud as unbacked, not silently assumed.
3. **Never guess conventions.** How this repo names things, wires dependencies, handles errors, or runs tests is discoverable in minutes. Guessing conventions is how changes arrive that are correct in isolation and wrong in the codebase.
4. **Trust code, not status.** A document's or ticket's self-reported state is not evidence of execution state; adjudicate with the code and the history (`git log -S <symbol>`, grep the tree) before building on it. Measured on one real backlog import, roughly half the self-reported statuses were stale.
5. **Reproduce before fixing.** For bugs: see the failure happen before changing anything. A fix for an unreproduced bug is a guess wearing a diff.
6. **Look for the landmines.** The output of grounding is a map: the touchpoints, the current behavior (quoted), and the invariants a change must not break. Tests named after old bugs, guards with explanatory comments, and constants encoding hard-won thresholds are the scars that mark where the landmines were.

## What grounding is not

- Not reading everything: map what the change touches plus one ring around it, at the depth the risk demands.
- Not a substitute for asking: when the code cannot answer an intent question (why is this threshold 7?), the history or the owner can; an unanswerable question becomes a named assumption, never a silent one.
- Not re-grounding what this session already established: ground once, cite it after.

## Common mistakes

- Theorizing from the framework's documentation about what the project's code does. The project forked, wrapped, or misused the framework; the tree tells you which.
- Grounding the happy path only. The invariants live in the error paths and the edge-case guards.
- Trusting a prior session's summary of the code over the code. Summaries route; the tree decides.
- Skipping grounding because the task "looks like" a previous one. The signal that pattern-matches a known case may have a different cause; check that the evidence supports this case.
