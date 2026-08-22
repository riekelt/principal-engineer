---
name: testing-changes
description: Use when deciding what tests a change needs - a feature, a bug fix, a refactor, any behavior change - or when reviewing whether a diff's tests are sufficient. Encodes tests-change-with-behavior, the bug-regression pattern, and assertion discrimination. Use whenever behavior changes and the test diff is empty, especially when the change is "too small to test".
---

# Testing changes

**REQUIRED BACKGROUND:** the `principal-engineering` skill. The craft of the tests themselves lives in `writing-unit-tests`; this skill governs which tests a change owes.

## Overview

A test is the executable form of a claim about behavior. A change that alters behavior without touching tests is a claim nobody wrote down. Two failure modes follow: the green suite over code that could not work, and the test or gate that turned out never to run.

## Tests a change owes

1. **Tests change with behavior, in the same change.** An empty test diff on a behavior change is a review finding, not a style preference. A pure refactor owes the opposite proof: the existing tests still pass unmodified, which is what makes it a refactor.
2. **A bug fix ships its regression test.** The test is named after the failure mode, not the ticket. The author shows both halves: red against the unfixed code, green against the fix. A regression test that never went red proves only that it compiles.
3. **Scenarios are concrete and include the surfaced edges.** The edge cases that grounding and review turned up go into tests by name; the happy path alone tests the demo, not the change. When the change's risk is in the failure path, the failure path gets the tests (see `handling-failures`: it requires a logged, typed failure, and that surfacing is behavior a test owes).
4. **Every task carries its targeted verify command.** The task names the verify command that proves this change, runnable alone and stated where the reviewer can run it. "The suite passed" vouches for nothing the suite never covered.
5. **Assertions must discriminate.** A test that passes regardless of the change proves nothing. Break the code once and confirm the test goes red, then restore it. Non-discriminating assertions are how suites stay green over broken behavior.
6. **Aggregates that must reconcile get invariant tests.** Anything on the project's declared critical paths (see the risk tiers in `principal-engineering`) that sums, derives, or mirrors other data gets more than point examples. The test asserts the reconciliation itself (the conservation pattern): the aggregate equals what the raw records imply.

## The red-test rule

A red test in a gate you own gets fixed, never silenced: weakening the assertion, deleting the test, or marking it skipped to ship is converting a detected defect into an undetected one. Changing the test is legitimate exactly when the test asserted the old, wrong behavior, and the change says so explicitly. You attribute the origin first, then fix the test regardless of whose it is; "pre-existing" is a footnote, never an excuse (see `verifying-before-done`).

## Tests a change does not owe

- Tests for unreachable edges (see `scoping-changes`: fencing what cannot happen is dead code with good intentions).
- Tests of framework internals or generated code. Test your use of them at the boundary you own.
- A test-first process: whether tests come first is workflow (a test-first workflow skill, where the project installs one, governs that); this skill governs what must exist when the change ships, whichever order produced it.

## Common mistakes

- "Too small to test." A one-line change reaches behavior no test covers, and the regression stays silent until someone audits it.
- Testing the fix without reproducing the bug. Red-before-green is the half that proves the test sees the defect.
- Counting coverage by feel. Count the changed behaviors against the tests naming them (the same counting rule as "Coverage is counted, not felt" in the technical-writer plugin's `technical-writing/references/truth.md`).
- Adding the test that discriminates against nothing, ever: an assertion no plausible defect could fail. Distinct from the legitimate pinning test that deliberately passes against both old and new code to guard unchanged adjacent behavior from overcorrection; a pinning test says that is what it is for.
