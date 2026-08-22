---
name: adding-dependencies
description: "Use when about to add, update, vet, or remove a dependency - a package, library, SDK, GitHub action, base image, or vendored code - or when a project's dependency posture needs declaring. Encodes the exhaust-what-you-have ladder, the vetting questions, and pin-and-prove updating. Use even for a tiny utility package: that is exactly how the tree grows."
---

# Adding dependencies

**REQUIRED BACKGROUND:** the `principal-engineering` skill.

## Overview

A dependency is a hire, not a snippet: with the feature come its defects, its release rhythm, its transitive tree, and its maintainer's attention span. Core principle: **exhaust what you already have, vet what you take, pin what you took, and record why.**

## Exhaust what you already have

Work down the list and stop at the first level that holds; good answers often compose two levels, an existing dependency for the hard part with a few lines of your own around it:

1. **The need itself.** Speculative need is no need; skip it and say so in a line.
2. **This codebase.** A helper, type, or pattern a few files away does the job; reuse it. Writing a second copy of something the repo already contains is the same defect `keeping-one-source-of-truth` bans for data.
3. **The standard library.** Read its index before installing a package that duplicates it.
4. **The platform.** A database constraint over application code, a native control over a widget library, the runtime's own primitive over a wrapper.
5. **A dependency the project already carries.** Its transitive tree is already paid for. Two boundaries sit here. A transitive you start using is a NEW direct dependency: declare it, pin it to the already-resolved version, and vet it lightly since the code already ships. Check the class the reuse crosses too, because a dev-tool's transitive promoted into runtime shifts the cost to every consumer's install, not just CI.
6. **A few lines of your own.** Owning twenty lines beats owning a stranger's repository, when twenty lines is truly all it takes.
7. **Only past all six:** a new dependency, vetted below.

## Vetting the one you take

- **Cost the whole hire**: the transitive tree it drags in, the install and build weight, the license against the project's, the security history (advisories, and a supply-chain score where a scanner runs).
- **Check the pulse**: recent releases, how issues get answered, how many people can merge. A load-bearing package with one exhausted owner is a risk you are choosing.
- **Read the part you will call.** Grounding applies to other people's code too: the API surface you depend on and its failure modes, not the README's promises.
- **Prefer the tool with one job** over the framework with forty; the other thirty-nine come along anyway, in weight and in attack surface.
- **Record the decision.** A new dependency is a decision: what it is for, what else was weighed, and the condition under which it leaves (via `recording-decisions` where installed). A tree full of unexplained packages is a decision log nobody wrote.

## Dependency posture

Dependency tolerance is the project's to declare, like its risk tiers: fully self-contained (some apps rightly ban external code wholesale), a curated allowlist, or vet-and-add. The project's rules or CLAUDE.md state which; when nothing does, ask what the system must never depend on and treat the answer as the declaration. A posture is honored even when inconvenient; changing it is a recorded decision, not an npm install.

An undeclared posture is not a hard gate: when nobody can answer today, proceed under a stated assumed posture, record the assumption in the decision, and track the declaration question with an owner. The assumed-and-tracked middle keeps the deadline moving and leaves the assumption visible.

## Pin and prove

- Commit lockfiles and pin versions; the build that worked today must work tomorrow, and an unpinned `latest` in CI or a base image is a time bomb with someone else's clock.
- An update is a change like any other. Read the release notes BEFORE a major bump (breaking changes and migrations first). Run the tests against it (`testing-changes`). Give a major bump its own commit so the blame trail stays readable, while grouped patch bumps may travel together.
- Vendored code carries its origin and version in the tree; you cannot update what you cannot date.

## Removal

- A dependency whose job disappeared leaves in the same change that removed the job; a package kept "in case" is the speculative need from level 1, in reverse.
- When the dependency is down to one small call site, consider owning those lines instead.
- Unmaintained but load-bearing is a risk item with an owner and a plan, never a hope.

## Common mistakes

- The tiny-utility reflex. Trees do not grow by big decisions; they grow one small package at a time, each individually reasonable.
- A framework installed to call one function.
- Depending on a package's internals or private paths; only the public contract is a promise.
- Importing a transitive dependency as if it were yours: undeclared today, gone on the next lockfile refresh. If you need it, declare it.
- Adding a dependency to avoid reading the code already in the repo (level 2, skipped).
- Vendoring without provenance, then wondering which version the copy was.
- Updating everything at once and discovering which bump broke the build by bisecting your own weekend.
