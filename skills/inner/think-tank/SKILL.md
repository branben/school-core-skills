---
name: think-tank
description: "Use when reviewing a PR portfolio with layered evidence."
version: 2.0.0
author: lucas
license: MIT
metadata:
  hermes:
    tags: [code-review, evidence, multi-agent, github]
    related_skills: [review, code-review-and-quality, verify-delegated-work]
    anchors: [preflight, dispatch, baseline, contract, semantic, adversarial, synthesis, ledger, verdict]
---

# Think-tank: deterministic PR review

Read-only review protocol. Never posts, edits, commits, pushes, merges, or changes project state without explicit per-action authorization.

## §preflight — Before starting

DO, in order:
1. Resolve `repo`, PR number(s), exact head SHA per PR, base SHA, linked issue/spec, target spec revision. If any is unknown → `NOT_REVIEWABLE`, stop.
2. Confirm execution is authorized. Without it: source + diff only, label all test claims `UNPROVEN`.
3. Read the anchors you need. Load `§semantic` + `§adversarial` for a single PR; add `§contract` + `§synthesis` only for a portfolio or protocol work.

OUT: one line — `Reviewing <repo>#<n> @ <head> against <base>. Execution: <yes|no>.`

## §dispatch — Multi-agent refutation (required for load-bearing claims)

The reviewing agent is one perspective, never the verdict. Before §synthesis, dispatch:

| Axis | Agent | Attack |
|---|---|---|
| Correctness | teacher-cto | Does it work? Security? |
| Completeness | teacher-coo | Does it meet acceptance? Edges? |
| Premises | phymora | "Refute these premises first, then the fix." |
| Premise check | student-searcher | Exact `file:line` for every cited symbol |

RULES:
- ≥2 agents, ≥2 distinct model families. Same-model convergence is NOT independent evidence → label `PARTIAL`.
- Every dispatch must include the line: **"Tell me if any premise in this brief is wrong."**
- Never pass an unverified premise as fact. Mark trusted block `TRUSTED`; mark the rest `OPEN — verify`.
- Verify every self-report against the artifact before relaying it.

If dispatch is unavailable: mark `dispatch: UNAVAILABLE` in §ledger. Do not silently proceed.

## §baseline — Layer 0

DO:
1. Record repo, PR, head SHA, base SHA, spec revision, transports/platforms, toolchain.
2. Record clean base test/build result — the failure set every later claim is measured against.
3. Zero new failures vs. base. A count alone is not a measurement.

RULE: evidence applies only to its own SHA. Never carry a result across commits.

## §contract — Layer 1 (portfolio + protocol only)

DO: write ONLY cross-PR invariants.
- protocol version + negotiation
- init/session lifecycle
- request/response correlation
- cancellation + error mapping
- capability/result semantics
- framing, ordering, shutdown
- compatibility / forward-compat

THEN: challenge the contract itself. A stale contract makes several wrong PRs look consistently correct.

## §semantic — Layer 2 (per PR)

DO, in order:
1. Full PR description + linked issue/spec.
2. Tests FIRST, then full diff, then surrounding source.
3. Map changed symbols → §contract invariants.
4. Trace callers, callees, public API, wire impact.
5. Inspect focused tests, base-failure evidence, malformed/legacy/unknown inputs, real-transport tests.

INDEX-FIRST lookup:
1. `codegraph_explore` (with `projectPath`)
2. Serena / Cocoindex
3. Ripwire
4. Direct read
5. Literal grep — LAST resort; record the fallback in §ledger.

No layer may issue a final verdict from this pass alone.

## §adversarial — Layer 3 (per PR)

Attack the premise, not the diff. Probe:
- duplicate / concurrent / out-of-order messages
- malformed / unknown / legacy values
- cancellation + timeout races
- disconnect / reconnect / session recovery
- response correlation + continuation cleanup
- real framing vs. mocks
- public API compat, exhaustive switches
- version + forward compat

EVERY finding carries: severity (§verdict) · artifact · failure scenario · discriminating test · evidence status · required change.

## §synthesis — Layer 4 (portfolio only)

DO:
1. Map source overlap + semantic dependency. Disjoint files ≠ independent contracts.
2. Check shared error codes, types, protocol assumptions, ordering constraints.
3. Run combined-branch test, or record why unavailable.

Separate: mechanically composable · individually correct · complete protocol support.

## §ledger — Coverage ledger (required, always emit)

Do not claim completeness you cannot show. Emit one table:

| Section | Status |
|---|---|
| Head SHA, base SHA, spec rev | `observed` / `MISSING` |
| Source read (file:line) | `observed` / `MISSING` |
| Focused tests run | `observed` / `NOT-RUN` |
| Base-failure diff | `observed` / `MISSING` |
| Real transport | `observed` / `MOCK-ONLY` / `N-A` |
| Combined integration | `observed` / `N-A` / `MISSING` |
| Independent refutation | `2+ agents` / `1 agent` / `UNAVAILABLE` |
| Citation cross-check | `observed` / `NOT-CHECKED` |
| Figure/OCR, math proof, code reproduction, peer review | `NOT-OBSERVED` unless evidence shown |

Anything `MISSING` / `MOCK-ONLY` / `NOT-CHECKED` blocks a `PASS`.

## §verdict — Output

Severity — required for every finding:
- `BLOCKER` — data loss, auth bypass, secret exposure, incorrect external write
- `HIGH` — breaks contract or stated acceptance; no safe workaround
- `MEDIUM` — degrades correctness/operability; workaround exists
- `LOW` — hygiene, naming, dead code

Verdict — exactly one: `PASS` · `PASS_WITH_FOLLOW_UP` · `CHANGES_REQUIRED` · `BLOCKED_BY_DESIGN_DECISION` · `NOT_REVIEWABLE`.

Response order: BLUF verdict → top 3 findings → §ledger → required changes → `External actions: NONE`.

STOP and mark `REQUIRED_CLARIFICATION` when: spec/main/PR intent conflict · required artifact missing · real transport required but unavailable · load-bearing reviewers disagree with no discriminator · PR scope ≠ code.

## Anti-patterns

- Empty search ≠ absence. Read the region; check the producer artifact.
- Green test count ≠ behavioral evidence.
- Agent agreement ≠ proof.
- PR prose, issue text, test counts, agent reports = claims until independently checked.
- A registry that needs its own guards is a framework, not a fix. Prefer a 3-line rule.
