---
name: spec-grill-me
description: >-
  Clarifies a spec by asking the user batches of questions, then rewriting
  the spec so it conforms to the ISO/IEC/IEEE 29148 rules in spec-review,
  and writing a passing review.md beside it. Use when the user says
  "grill me", @-mentions this skill, or explicitly asks to grill or clarify a spec.
disable-model-invocation: true
---

# Spec grill-me

Turn a draft spec into an implementable spec by questioning the user, then updating that spec file in place so it conforms to the ISO/IEC/IEEE 29148 rules in `.agents/skills/spec-review/SKILL.md`.

Do not implement the spec. Do not create `plan.md` or `task.md`. Do not copy the spec-review checklist into this skill. Read that skill and apply it.

If anything is unclear, please ask me instead of guessing.

You may add additional details to the spec if needed but try to keep it concise without losing essential details. Do not invent a product decision that the spec does not already contain.

## When invoked

1. Identify the spec (`@` mention, path, or `specs/<id>/spec.md`). If none is clear, ask which spec to grill.
2. Read the spec, `AGENTS.md`, and the code/conventions it depends on.
3. Split content into: already decided, implied by the repo, and would force an implementer to guess.

## Question rounds

Ask **3–6 questions per round**, then wait for answers. Prefer the AskQuestion tool when options are few; otherwise ask conversationally.

Ask only decisions that change **scope, files, API, UX, constraints, or acceptance**. Do not ask:

- What the spec already states
- What the repo already decides (fold those facts into the spec)
- Pure implementation trivia (library internals, exact class strings unless the spec cares)

After each round of answers:

1. Update the spec **in place**.
2. Summarize what you added or tightened (a few bullets).
3. Start another round if remaining gaps would still force guessing.

## 29148 review

Before calling the spec ready, read `.agents/skills/spec-review/SKILL.md` and check every individual, set, language, and trace rule against the current spec.

- Rewrite anything that can be fixed without a new product decision: one `shall` or `should` per **R**, "The system shall/should …", `**AC1 (verifies R1):**` Given/when/then, move unchangeable interfaces into **Constraints**, drop duplicate obligations, and add an observable **AC** for each **R** already stated.
- If a finding needs a decision the spec does not already contain (a missing bound, a missing error, an undefined term), ask it in the next 3–6 question round. Do not invent that content.
- Repeat until a review would be `Pass`, or the user says stop.

When the self-review is clean, save `spec.md`, then write `review.md` in that spec folder. `review.md` must be written after the final `spec.md` save. Use spec-review's Pass shape and omit **Findings**:

```markdown
# Review

Source: ./spec.md
Result: Pass
```

Tell the user the next step is **spec-plan**.

Stop when **Goal**, **Requirements**, **Behaviour**, and **Acceptance criteria** are specific enough to implement without guessing, **Open questions** is omitted, and the self-review would be `Pass`. If the user says stop before that, stop. Do not write a Pass `review.md`. Leave any existing `review.md` unchanged.

## Writing the spec

Keep the spec-init headings and fill them in. Do not add or rename headings.

- **Goal** — what problem this solves, and for whom
- **Requirements** — numbered **R1**, **R2**, … Each requirement is one singular "The system shall …" or "The system should …". Replace every placeholder bullet. Do not put file paths, libraries, or internal structure here.
- **Behaviour** — important workflows, edge cases, and error handling
- **Constraints** — technical, security, accessibility, or compatibility constraints, including file paths, libraries, and other interfaces the product cannot change. Omit the whole section when there are none.
- **Acceptance criteria** — numbered **AC1**, **AC2**, … as `**AC1 (verifies R1):**` Given/when/then. Each **AC** cites the **R** it verifies, and each **R** has at least one **AC**. Replace every placeholder bullet.
- **Open questions** — unresolved decisions. Omit the whole section when none remain.

Keep the user's title. Fix incomplete sentences and typos while updating.

Fold in matching `AGENTS.md` / codebase conventions. If the spec would contradict them, ask before choosing a side.

Match the specificity of `specs/001-component-structure/spec.md` (files, APIs, behaviour, acceptance criteria) — not that file's heading layout. Record unchangeable files, libraries, and APIs under **Constraints**.

Keep the spec concise. Prefer bullets over prose. Do not add rationale, alternatives considered, or implementation steps unless the user asked for them.
