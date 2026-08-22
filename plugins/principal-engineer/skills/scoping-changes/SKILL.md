---
name: scoping-changes
description: Use when deciding how big a fix should be, when scope is drifting mid-task, when a plan is being quietly trimmed to fit, or when "while you're at it" appears in any form. Encodes size-to-trigger, decompose-not-descope, and drive-to-completion. Use whenever the work is about to grow past its cause or shrink below its promise.
---

# Scoping changes

**REQUIRED BACKGROUND:** the `principal-engineering` skill.

## Overview

Scope fails in both directions: gold-plating grows a fix past its trigger, and silent descoping shrinks an approved task below what was agreed. Both are the same defect: the delivered change no longer matches its cause. Core principle: **size fixes to their trigger, and when reality forces a cut, decompose visibly instead of trimming quietly.**

## Sizing to the trigger

- **The trigger is the defect or need itself, not the ticket's sentence about it.** The same defect discovered on a second path is in scope (shipping "fixed" while it lives on elsewhere is a half-true report); an adjacent improvement discovered because the file was open is not.
- The fix is as big as the thing that triggered it. No gold-plating, no fencing unreachable edges, no refactor riding along because the file was open.
- Adjacent improvements you noticed are real and belong in the tracker, not in this diff.
- The test for a borderline addition: would this change ship on its own merits if the main fix did not exist? If not, it is decoration on someone else's diff. If it would, it ships on its own: passing the test licenses a separate change, never a rider.
- The owner can re-scope; the owner cannot merge scopes. "Squeeze it in" from whoever owns the work legitimately adds the second task, and it still ships as its own change. These rules govern how work is shaped; the owner decides what work exists.
- Unrequested structure is scope creep wearing a design pattern: an interface with a single implementer, a factory that only ever builds one thing, configuration for a value nobody will change. Add the structure when the second case arrives, not when it is imagined.
- A deliberate simplification that accepts a real limit carries that limit in a comment: what the ceiling is and what would justify raising it ("one shared queue; shard per tenant when a single consumer can no longer keep up"). The ceiling is a current constraint, so the comment describes what the system does today, not the history of the decision.
- Choosing how to solve, not only how much: exhaust the codebase, the standard library, the platform, and the dependencies already installed before writing new code, and see `adding-dependencies` before reaching for a new package.

## Decompose, never silently descope

- The owner decomposes an approved task that turns out too big into named parts with the cut line stated, never delivering a quiet "pragmatic minimum" that looks complete.
- The author files what gets deferred as a tracked item with an owner, or explicitly "owner pending triage" when none exists yet. The tracker discipline lives in the technical-writer plugin's `writing-issues` where installed. Deferred work that lives only in the author's memory was descoped, not deferred.
- The report says plainly which parts shipped and which did not. A partial delivery honestly labeled is a plan; a partial delivery labeled complete is a defect.

## Driving to completion

- An approved plan runs to completion without "want me to continue?" checkpoints; stop only for genuine blockers, destructive actions, or hard gates that need the operator.
- Blocked on one part: finish the unblocked parts, surface the blocker with what it needs, never let one stuck task silently stall the rest.
- Settled decisions stay settled mid-execution. New information that genuinely reopens one becomes an explicit re-decision, not a quiet swerve. Record it via `recording-decisions` where the technical-writer plugin is installed.

## Scope in review

- Reviewing a change: an unrelated defect you noticed in passing is not your finding; note it once for the tracker and stay on the diff. Out-of-scope findings dilute the verdict and train authors to fear review.
- Being reviewed: findings against the diff get fixed or explicitly answered; findings outside the diff get tracked, not absorbed into the change.

## Common mistakes

- "While I'm here" as a justification. You are here for the trigger.
- Descoping to hit a deadline and reporting done. The deadline pressure was real; the honest move was decomposing and saying which half shipped.
- Fencing edge cases the system cannot reach, to feel thorough. Unreachable defensiveness is dead code with good intentions.
- Re-litigating an approved design in the middle of implementing it because a mildly better idea appeared. Write the idea down; finish the plan; propose the idea against the shipped reality.
- Letting a reviewer's out-of-scope wish expand the diff. Track it, thank them, ship the trigger.
