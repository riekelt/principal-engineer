---
name: scoping-changes
description: Use when deciding how big a fix should be, when scope is drifting mid-task, when a plan is being quietly trimmed to fit, or when "while you're at it" appears in any form. Encodes size-to-trigger, decompose-not-descope, and drive-to-completion. Use whenever the work is about to grow past its cause or shrink below its promise.
---

# Scoping changes

**REQUIRED BACKGROUND:** the `principal-engineering` skill.

## Overview

Scope fails in both directions: gold-plating grows a fix past its trigger, and silent descoping shrinks an approved task below what was agreed. Both are the same defect: the delivered change no longer matches its cause. Core principle: **size fixes to their trigger, and when reality forces a cut, decompose visibly instead of trimming quietly.**

## Sizing to the trigger

- The fix is as big as the thing that triggered it. No gold-plating, no fencing unreachable edges, no refactor riding along because the file was open.
- Adjacent improvements you noticed are real and belong in the tracker, not in this diff. One trigger, one change, one reviewable story.
- The test for a borderline addition: would this change ship on its own merits if the main fix did not exist? If not, it is decoration on someone else's diff.

## Decompose, never silently descope

- An approved task that turns out too big is decomposed into named parts with the cut line stated, never delivered as a quiet "pragmatic minimum" that looks complete.
- What gets deferred becomes a tracked item with an owner (the tracker discipline lives in the technical-writer plugin's `writing-issues` where installed); deferred work that lives only in the author's memory was descoped, not deferred.
- The report says plainly which parts shipped and which did not. A partial delivery honestly labeled is a plan; a partial delivery labeled complete is a defect.

## Driving to completion

- An approved plan runs to completion without "want me to continue?" checkpoints; stop only for genuine blockers, destructive actions, or hard gates that need the operator.
- Blocked on one part: finish the unblocked parts, surface the blocker with what it needs, never let one stuck task silently stall the rest.
- Settled decisions stay settled mid-execution. New information that genuinely reopens one becomes an explicit re-decision (recorded, where the technical-writer plugin is installed, via `recording-decisions`), not a quiet swerve.

## Scope in review

- Reviewing a change: an unrelated defect you noticed in passing is not your finding; note it once for the tracker and stay on the diff. Out-of-scope findings dilute the verdict and train authors to fear review.
- Being reviewed: findings against the diff get fixed or explicitly answered; findings outside the diff get tracked, not absorbed into the change.

## Common mistakes

- "While I'm here" as a justification. You are here for the trigger.
- Descoping to hit a deadline and reporting done. The deadline pressure was real; the honest move was decomposing and saying which half shipped.
- Fencing edge cases the system cannot reach, to feel thorough. Unreachable defensiveness is dead code with good intentions.
- Re-litigating an approved design in the middle of implementing it because a mildly better idea appeared. Write the idea down; finish the plan; propose the idea against the shipped reality.
- Letting a reviewer's out-of-scope wish expand the diff. Track it, thank them, ship the trigger.
