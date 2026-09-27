<p align="center">
  <img src="assets/logo.png" alt="School Core" width="260">
</p>

<p align="center">
  Agent skills for running a two-loop SDLC — epic to tickets, ticket to merged PR.
</p>

---

# school-core-skills

Sixteen agent skills for the **outer loop** (epic → PRD → SPEC → tickets) and the
**inner loop** (Prime → Plan → Implement → Validate → Review → PR) that turns one
ticket into a merged pull request.

## Install

### With `npx skills` (works for any agent)

```bash
npx skills add branben/school-core-skills
```

Add `-g` to install globally, or install a single skill:

```bash
npx skills add branben/school-core-skills -g
npx skills add branben/school-core-skills to-tickets
```

Requires a repo with a readable history — several skills read merged PRs or
commit log to work out what a project actually values.

> **Installing overwrites.** `npx skills add` replaces any existing skill of the
> same name, with no prompt and no diff. If you've edited your copy of a skill
> in this repo, **diff before you install** — otherwise the edit is gone.
> Installs are tracked in `skills-lock.json`; `npx skills remove` undoes one
> (bare `npx skills remove` gives you a picker).
>
> To pull in a single skill without writing any files, use the `@` form —
> `npx skills use branben/school-core-skills@to-tickets` — which prints the
> skill for the agent to follow. Two things that do **not** work:
> `npx skills add branben/school-core-skills to-tickets` installs all 16, and
> `npx skills use branben/school-core-skills to-tickets` errors with
> "Expected one source, received 2".

## Layout

Skills live under `skills/`, grouped by loop:

- `skills/outer/` — epic → tickets, the human-driven planning loop
- `skills/inner/` — ticket → merged PR, the build loop
- `skills/shared/` — usable in either loop

Each skill is its own directory containing a `SKILL.md` (YAML frontmatter with
`name` and `description`) plus any bundled scripts, references, or assets.

## The two loops

The **outer loop** turns an epic into tickets. It is human-driven: an epic
arrives, a PRD and SPEC get written and reviewed, and tickets land with their
blocking edges declared.

```
grill-me → vision → to-prd → to-spec → to-tickets
```

The **inner loop** turns one ticket into a merged PR. Every slice walks it start
to finish, and a failed sub-step loops back to *that* step, not the whole slice.

```
Prime → Plan → Implement → Validate → Review → PR
```

The two loops share a rendering layer. The [plannotator](https://github.com/plannotator)
skills carry **5 outer phases** — pre-PRD exploration, PRD, PRD alternatives,
SPEC, ticket — and **5 inner phases** — prime, plan, validate, review, PR. That
overlap is why they are bucketed by *when you need them* rather than by loop.

They meet at one ritual: pick one ticket, state "done means" out loud, work it,
validate, review, write the bead. One ticket at a time — a failed slice is a
lesson, not a reason to run two.

## Reference

### Outer

- **grill-me** — Interviews you relentlessly about a plan or design, one question
  at a time, resolving each branch of the decision tree and giving a recommended
  answer for each. Explores the codebase instead of asking you what the code says.
- **vision** — Mines what you actually build (merged PRs, commit history) and
  drafts a `VISION.md` as a *testable acceptance policy*, then stress-tests it
  with fault-line hypotheticals whose answers only you can give. Refuses to invent
  values it can't cite evidence for.
- **to-prd** — Turns the current conversation into a PRD and publishes it to the
  issue tracker. No interview — just synthesis of what you've already discussed.
- **to-spec** — Same discipline, producing a spec: decisions first, scope, and an
  explicit non-goals list.
- **to-tickets** — Breaks a plan, spec, or PRD into tracer-bullet vertical slices,
  each declaring the tickets that block it, and publishes them in dependency order.
  Ships hard-won `gh` CLI pitfalls that silently waste an afternoon otherwise.

### Inner

- **html-diagram** — Architecture, sequence, process, and state diagrams as
  self-contained HTML.
- **html-plan** — Plans with milestones, sequences, and dependencies, where the
  markdown source stays canonical and HTML is the render.
- **html-wireframe** — Low-fidelity, monochrome structure comparisons — the
  right fidelity for deciding between layouts.
- **html-prototype** — Working flows with every real state modelled: loading,
  empty, error, success, disabled, mobile, reduced-motion.
- **html** — Broad HTML deliverables: reports, explainers, landing pages, decks.
- **design-artifact** — Design direction for any visual HTML deliverable: palette,
  type pairing, layout, theming, and the specific tells that make output look
  generically generated. Ships anti-patterns, linking, and publishing references.
- **plannotator-guide** — A guided review as a chaptered walkthrough of a change,
  rather than a wall of findings.
- **round-based-hook** — Boxing-round responses: what landed, where the damage
  fell, what was traded away, a STE summary, and a plain-language read. Carries a
  mastery ladder, so explanations get simpler when you need them to and sharper
  when you don't.
- **think-tank** — Read-only, multi-agent review of a PR portfolio with layered
  evidence. The reviewer is one perspective, never the verdict; load-bearing
  claims get dispatched for refutation before synthesis.
- **chunked-writing** — Shapes long output: a BRIEF for the reader who arrived
  cold, then a BLUF for the reader in a hurry, then a chunked body a reader can
  navigate by headings alone.

### Shared

- **implement** — Carries a plan, spec, or ticket into working code.

## Credits

These skills are assembled from work by other people, and the repo would not
exist without them. Each is credited to its source:

| Skill | Upstream | License |
|---|---|---|
| `to-prd` `to-spec` `to-tickets` `grill-me` | [mattpocock/skills](https://github.com/mattpocock/skills) | MIT © 2026 Matt Pocock |
| `vision` | [kunchenguid/vision](https://github.com/kunchenguid/vision) | MIT © 2026 kunchenguid |
| `html` `html-plan` `html-wireframe` `html-prototype` `html-diagram` | [plannotator/effective-html](https://github.com/plannotator/effective-html) | MIT |
| `plannotator-guide` | [plannotator/guides](https://github.com/plannotator/guides) | not stated — see note |

**plannotator** — [@plannotator](https://github.com/plannotator) — builds the
render-and-review side of this workflow. `effective-html` covers the artifact
skills (MIT); `guides` covers the guided-review skill. Please credit them if you
fork this.

> **Note on `plannotator-guide`.** `plannotator/guides` publishes no LICENSE file
> and no license metadata, so its redistribution terms are genuinely unclear —
> not merely undocumented on my part. It is included here with attribution as
> you asked. If you would rather not ship content of unclear provenance, delete
> `skills/inner/plannotator-guide/`; nothing else depends on it.

`to-spec`, `to-tickets`, and `grill-me` carry local edits on top of upstream —
extra CLI pitfalls, a provenance footer, and a self-contained interview
procedure. **Do not re-sync them from upstream without diffing first**, or you
will silently lose that work.

## License

MIT — see [LICENSE](LICENSE). Third-party skills remain under their own
licenses, listed above.
