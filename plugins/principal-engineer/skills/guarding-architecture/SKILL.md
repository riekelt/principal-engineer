---
name: guarding-architecture
description: "Use when a change crosses module boundaries, adds a dependency direction between modules, touches a critical path, or conflicts with a stated principle - and when writing or updating architecture principles themselves. Encodes structural invariants as named, enforced contracts: statement, rationale, guard. Use whenever \"we'll just import it from there for now\" appears, which is how boundaries die."
---

# Guarding architecture

**REQUIRED BACKGROUND:** the `principal-engineering` skill.

## Overview

Structural invariants are load-bearing contracts: violating one surfaces as a class of bugs, not a single defect. Core principle: **an invariant that matters gets a name, a written rationale, and a mechanical guard; an invariant without a guard is a wish.**

## The pattern

1. **Name the invariants.** Numbered and citable ("Law 3"), each with a Statement (technology-neutral, meant to outlast any framework), a Rationale, and Implications. The rationale is a concrete failure narrative: the class of bugs that appears when the invariant is violated, told from an incident, not an abstraction. An invariant whose rationale nobody can state is one nobody will defend.
2. **Split the stable from the volatile.** The principles document changes rarely and names no classes; its current realization (the canonical owners, the guards, the reference designs) lives in a companion that changes with the code. When the two disagree, the invariant governs and the realization document gets corrected. The split keeps principles documents free of stale class names.
3. **Enforce mechanically.** Every enforceable invariant gets an architecture test that fails the build: dependency directions, package boundaries, layering rules, forbidden imports. What cannot be build-enforced becomes a named review check with the invariant cited. An invariant enforced by memory is enforced until the person who remembers it is on holiday.
4. **Violations mean redesign, never justification.** A design that violates a named invariant is wrong by construction: redesign it, do not argue the exception into the spec. Watering the contract down to match nonconforming code is the banned move; the violation gets recorded and the code gets fixed (the same rule the technical-writer plugin applies to normative documents).
5. **Specs show conformance.** A design that touches guarded ground names the invariants it touches and shows, per invariant, how it upholds each. This makes architecture review objective: the reviewer checks claims against named invariants instead of debating taste.
6. **Exceptions are amendments.** A genuine exception proposes an amendment, naming the invariant it bends and the boundary of the bend; silent exceptions are how an invariant becomes a suggestion. This is the only legal form of exception, and point 4 bans every other; an unreachable owner does not create one, so the change waits or lands conforming. A genuinely temporary exception is a dated waiver with an expiry condition and the owner's sign-off, recorded in the volatile realization document rather than by amending the stable invariant for a passing condition.
7. **An unexplained guard exclusion is a violation hidden from the build.** Whoever finds one surfaces it to the invariant's owner. An exclusion is never precedent for the next one, and extending an exclusion list "like the others did" ratifies erosion instead of following a pattern.

## Common invariant classes

Instances worth guarding in most systems, as examples rather than mandates:

- One canonical owner per concern (see `keeping-one-source-of-truth`).
- Dependency direction: the domain never imports the delivery mechanism.
- Critical-path isolation: no I/O and no slow or optional dependency on the hot path.
- Fail-closed boundaries: a gate that cannot evaluate must deny (see `handling-failures`).
- Migration immutability (see the hard rules in `principal-engineering`).

## Common mistakes

- A principles document full of class names. That is the realization document wearing the wrong title; split them.
- Adding the import "for now". Boundaries die by single convenient imports; the guard exists because each violation is locally reasonable.
- An invariant asserted in review but absent from the build. It will be enforced exactly as often as the right reviewer is present.
- Justifying a violation by the cost of conforming. The cost argument may be right, but its correct form is an amendment to the invariant, decided by the owner, recorded (via `recording-decisions` where installed), never a quiet exception in one spec.
- Principles written as taste ("prefer small modules") instead of contracts ("module X never imports module Y"). A contract can fail a build; taste can only fail a mood.
