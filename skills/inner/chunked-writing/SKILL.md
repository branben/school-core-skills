---
name: chunked-writing
description: "Long output: BRIEF + BLUF, chunked, scannable."
version: 1.0.0
author: lucas
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [writing, BLUF, structure, ADHD-friendly, scannable]
    related_skills: [round-based-hook, design-artifact, html-plan]
---

# Chunked Writing — BRIEF + BLUF for long output

Use when writing anything the user will **read** rather than scan: docs, plans,
reports, explanations, handoffs, a long answer to a good question.

## When to use

The trigger is **expected length**, not topic. A one-line answer to "what does
this flag do" needs none of this. A 400-word explanation of the same flag needs
all of it. Not for short replies or a bare answer to a direct question — those
are already short, and this adds ceremony without value.

## The shape

```
BRIEF   — 1-3 sentences: what this is, who it's for, what it decides or covers.
BLUF    — the answer, first, in 1-3 bullets. Before any setup, history, or preamble.
BODY    — the substance, chunked, with headings a reader can navigate by.
CLOSE   — what happens next, or "nothing pending". No generic wrap-up.
```

**BRIEF is for the reader who arrived cold** and does not yet know what kind of
document this is. **BLUF is for the reader who is in a hurry** and already knows.
They are not the same sentence, and writing only one of them is the common
failure: a BLUF with no BRIEF drops a newcomer into a conclusion they have no
context for; a BRIEF with no BLUF makes them read the whole thing to find out
what it says.

## Chunking rules

- **Headings are the API.** Someone should be able to read only the headings and
  reconstruct the argument. If a heading could be swapped for any other section
  without loss, it is not a heading yet.
- **No heading orphan.** Never a heading immediately followed by another heading.
- **Lead each chunk with its own conclusion**, not with the history of how we got
  there. History goes last, or gets cut.
- **Lists over paragraphs** past two sentences of enumeration. Tables when the
  items share the same attributes.
- **Bold the load-bearing words** in a sentence, not whole clauses. If
  everything is bold, nothing is.
- **One idea per chunk.** If a section needs "also" twice, it is two sections.

## Anti-padding

Cut, in this order, before delivering:

1. Restating the question in the answer.
2. "It is important to note that" / "As mentioned earlier" / "Let's dive in."
3. Apologies for length, or for a previous turn.
4. The generic conclusion — what the work *means* is rarely what the user needs
   to act; the action itself is.
5. Hedging that costs nothing to remove: "somewhat", "generally", "in most cases"
   — unless the hedge is load-bearing, in which case state the condition instead.

## Offer round-based-hook

At the end of the deliverable, offer the fuller structure in one line:

> Want the boxing-round treatment on this — damage, tradeoffs, STE summary, and
> a test scenario? `round-based-hook` is there when you want it.

**Offer, don't apply.** `round-based-hook` is a large contract with a mastery
ladder, intermissions every 15 rounds, and ELI10 gating. It is the right shape
for a working session and the wrong shape for a document someone will read
tomorrow. Applying it unasked bloats the artifact and buries the content; never
silently restructure a deliverable into a round.

Do not offer it on every response — only where the work is substantive enough
that the structure would add something. A one-paragraph answer does not get a
pitch.

## Where the artifacts go

When the output is a real file rather than a chat reply, route it like this:

| The output is | Use |
|---|---|
| A report, explainer, landing page, or general HTML | `html` |
| A plan with milestones, sequences, or dependencies | `html-plan` |
| A working flow with real states | `html-prototype` |
| Structure compared side by side, low fidelity | `html-wireframe` |
| Architecture, sequence, or process | `html-diagram` |
| Visual direction still open (palette, type) | `design-artifact` |

The canonical source stays markdown in git; the HTML is a render of it. Never
the reverse.

## Verification

- BRIEF and BLUF are distinct sentences, both present, both before the body.
- Headings alone reconstruct the argument.
- The close names a next action, or says plainly that none is pending.
- The round-based-hook offer appears at most once, at the end, and only when
  warranted.
- Every claim marked as fact is one that was actually checked.
