---
name: keeping-one-source-of-truth
description: "Use when adding data, config, state, constants, an enum-like string, a cache, or anything that could exist in two places - or when two sources already disagree. Encodes the one-fact-one-home doctrine for code and data: derive rather than store, extend the owner, absorb duplicates. Use at the moment copying a value feels faster than referencing it."
---

# Keeping one source of truth

**REQUIRED BACKGROUND:** the `principal-engineering` skill.

## Overview

Every fact about the system is described in exactly one place, and everything else reads it. This outranks convenience: a second copy is a future contradiction, and the copy that drifts is always the one nobody remembers exists. When two places can hold the same truth they will eventually disagree, and the system then looks healthy while serving wrong data.

## The doctrine

1. **Before adding data, find who already owns it.** Extend that owner; do not start a rival. The five minutes of finding the owner is cheaper than the eventual incident of two owners.
2. **Derive rather than store.** If the platform or an existing source can answer it at read time, read it there; do not copy the answer into a second home where it can go stale.
3. **Absorb duplicates you find on the way.** Touching code that hardcodes what a file already knows (or the reverse) means folding the two together as part of the work, not leaving a third variant behind.
4. **A missing entry fails loud** (see `handling-failures`): the single source is only authoritative if absence from it is an error, never a silent default.
5. **Mark generated versus hand-edited, and never edit generated output.** Every artifact states which it is; edits to derived files are lost work plus a divergence.
6. **Vocabulary is typed, not stringly.** Identifiers, kinds, states, and names that code branches on are constants, enums, sealed types, or registry entries; a free string spelled twice is two sources of truth with a typo between them.
7. **When two sources disagree, say so.** One of them is stale; surfacing the contradiction is the first fix, and the full fix determines which value is live and collapses to one home with the loser deleted. Silently following either one launders the disagreement into whichever answer you happened to read first. On a declared critical path, a live disagreement earns a direct message to the owner, not only a tracked item; an unread ticket surfaces nothing.

## Boundaries

- Caches and read models are legitimate derived copies when their derivation is automatic and their staleness is bounded and observable. The rule bans hand-maintained copies, not architecture.
- Test fixtures may freeze a copy of reality on purpose; a fixture is a snapshot, labeled by being a fixture.
- Documentation follows the same rule (an index routes, never decides); the technical-writer plugin carries that side where installed.

## Common mistakes

- Copying a threshold, URL, or mapping "temporarily". Temporary copies have the same lifetime as the TODO above them.
- Creating `thing-v2` beside `thing` instead of editing in place. The second file is a fork of the truth, and both will receive different fixes.
- A default value in code that shadows the config file's value. When someone changes the config and nothing happens, this is why.
- Two enums in two services spelling the same states. The day one gains a state, the boundary between them becomes a silent filter.
