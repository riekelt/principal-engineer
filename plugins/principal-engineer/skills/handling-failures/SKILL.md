---
name: handling-failures
description: Use when writing or touching any error path, catch block, fallback, default value, retry, or degraded mode - in any language, any repo. Encodes the no-silent-swallows contract and the fail-loud discipline. Use whenever an exception is about to be caught, a null is about to get a default, or a failure could pass unnoticed, even if the goal is "just make it not crash".
---

# Handling failures

**REQUIRED BACKGROUND:** the `principal-engineering` skill.

## Overview

A swallowed error is a bug with its evidence destroyed. Core contract: **every failure path does exactly one of three things, and all three are loud.** A system that looks healthy while serving wrong data is worse than one that crashes.

## The contract

Every catch and failure path either:

1. **Logs at WARN or ERROR and rethrows**, or
2. **Logs and returns a TYPED failure the caller must handle** (a result type, a sealed error, a status the compiler or contract forces downstream code to acknowledge), or
3. **Logs and enters an explicitly documented degraded mode** (named in the code and its documentation, with something observable saying the system is degraded).

Forbidden, no exceptions:

- Bare catch-and-continue.
- Catch-and-return-default (empty list, null, zero, cached copy) that masks the failure.
- `?? fallback` and its cousins where the fallback hides that the primary failed. A fallback is acceptable only when the absence is ALSO surfaced loudly elsewhere.

## Corollaries

- **A missing required entry fails loud.** Something absent from a registry, config, or catalog is a build or startup failure, never a silent default.
- **Error states are visible.** A workflow must not appear healthy while failing; surface the error state in the UI, the metrics, or the logs someone actually watches.
- **Operator-facing remediation is specific.** "connection failed, check REDIS_URL and whether redis responds to PING" beats "an error occurred".
- **Retries are bounded and observable.**
- **Replays of side-effecting operations are idempotent.**
- **Degraded modes have a bound.** Skip-and-continue needs the explicit threshold where degradation becomes abort, as a named, operator-tunable constant. The guard must exist; its value is a judgment call to make with the owner. An unbounded degraded mode is a slow-motion swallow.
- **On failure paths, observability is part of the minimum**, not gold-plating. The log line, the counter, and the alert ship with the fix; a failure path without them is the silent swallow with better intentions.

## Touching existing swallows

Code you are editing that already swallows: fix it as part of the work, or explicitly flag it as owed with what it hides. Leaving it silently is endorsing it. In review, a NEW silent swallow is an automatic BLOCKER; a pre-existing one you touched and left unflagged is a WARNING against the change.

## Common mistakes

- "It should never happen" as a reason to swallow. Those paths are exactly the ones that need a loud alarm when they do.
- Logging at DEBUG and calling it handled. If nobody sees it in production, it is a swallow with extra steps.
- Catching broad (`Exception`, `catch {}`) to handle narrow. The unexpected failure dies silently beside the expected one.
- A degraded mode nobody documented. Degradation only the author knows about is an outage the operator cannot diagnose.
- Making the test pass by defaulting the failure. The test goes green; the defect graduates to production.
