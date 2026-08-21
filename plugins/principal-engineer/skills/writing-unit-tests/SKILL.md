---
name: writing-unit-tests
description: "Use when writing or refactoring unit tests - a new test file, added cases, a flaky test, an unreadable one. Encodes behavior-first testing: one behavior per test, names that document, deterministic setup, mocks only at boundaries you do not own. Use whenever a test is being written, even a quick one, and whenever a test needs a sleep, a mock of your own code, or a copy of the implementation's math."
---

# Writing unit tests

**REQUIRED BACKGROUND:** the `principal-engineering` skill. Which tests a change owes is `testing-changes`; this skill is the craft of the tests themselves.

## Overview

A unit test is a behavioral claim with a name, read by the next engineer during a red build. Core principle: **test the contract, not the implementation; make the name carry the claim; and keep the test so simple it cannot itself be wrong.**

## Behavior, not implementation

- Test through the public contract of the unit. A refactor that preserves behavior should not break tests; when it does, the tests were asserting the implementation, and they now punish improvement.
- Do not assert call sequences, internal state, or that method A called method B, unless the interaction IS the contract (a required side effect on a boundary). Asserting internals tests the implementation twice and the behavior zero times.
- Never derive the expected value from the production arithmetic, neither by reimplementing the formula nor by invoking the shared helper that computes it. Both prove the code equals itself, and both stay green when the shared code carries the bug. Expected values are literals worked out independently (by hand, from a spec, from real data), with the derivation in a comment.

## One behavior per test, named as the claim

- One behavior per test; splitting is cheaper than archaeology on a multi-assert failure.
- The name states subject, scenario, and expected outcome: `expired_token_is_rejected_with_401`, not `test_auth_3`. Test names describe behavior, state transitions, and invariants; never delivery order, ticket keys, or phases. Read the test list of a module and you have read its spec; that is the bar.
- Arrange, act, assert, visibly and in that order. No branching, loops, or logic in a test: a test with logic needs its own test. Shared setup earns a builder or a role-named fixture; a mystery blob fixture hides which arranged fact the assertion depends on. Generation and iteration live in builders and helpers, not in the test body; property-based tests are the accepted form for invariants and follow their framework's shape, while example-based tests stay logic-free.

## Determinism

- No real time, real randomness, real network, or real filesystem inside a unit test: inject the clock, seed or inject the randomness, fake the boundary. The test that passes at 14:00 and fails at midnight is a bug report about the test.
- No sleeps. Waiting for async work is condition-based (poll the observable outcome with a deadline), never duration-based; a sleep is a race condition with a timer attached.
- A flaky test is red: fix it or quarantine it visibly with an owner (see the red-test rule in `testing-changes`); re-running until green is silencing a detector.

## Mocks are assumptions

- Mock the boundaries you do not own (network, clock, filesystem, third-party services); prefer real collaborators for code you do own within the unit's reach. For owned wrappers around unowned resources (your repository class fronting the database), fake at the seam where owned code last touches the unowned resource, and keep the test data role-named and visible either way. Every mock hardcodes an assumption about a contract, and a stale mock is how a suite stays green while the real integration is broken.
- When a test is mostly mock wiring, it is testing the mocks. Either widen the unit to something with real behavior or accept that this seam needs an integration test instead (and say which).
- Fixtures are labeled snapshots of reality: minimal, role-named for their part in the scenario, and updated deliberately when the contract changes, never regenerated blindly to make red go green.

## Assertions and failure paths

- Assert outcomes with values, not absence of exceptions. "It did not throw" claims almost nothing.
- Failure paths are first-class test subjects: the typed failure surfaces, the degraded mode is entered loudly, the guard actually guards (see `handling-failures`). The error path without a test is the silent swallow's favorite hiding place.
- Tests themselves follow the no-silent-swallows contract: no catch-and-ignore in test code, no conditional assertions that skip silently when a precondition is absent; a test that cannot run must fail or be visibly skipped with the reason.

## Common mistakes

- The mirror test: reimplementing the production logic to compute the expectation.
- The mock echo chamber: stubbing your own class and asserting the stub.
- The mega-test: twelve assertions, one name, no way to know which claim broke.
- Shared mutable fixtures that make test order matter; every test builds or receives its own state.
- The sleep that "fixes" flakiness by making it rarer.
- Green-checking the fixture: editing expected values to match actual output without deriving why the new value is right.
