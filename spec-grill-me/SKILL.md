---
name: spec-grill-me
description: >-
  Clarifies a spec by asking the user batches of questions, then writing
  detailed requirements back into the spec file. Use when the user says
  "grill me", @-mentions this skill, or explicitly asks to grill or clarify a spec.
disable-model-invocation: true
---

# Spec grill-me

Turn a draft spec into an implementable spec by questioning the user, then updating that spec file in place.

Do not implement the spec. Do not create `plan.md` or `task.md`.

If anything is unclear, please ask me instead of guessing.

You may add additional details to the spec if needed but try to keep it concise without losing essential details.

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

Stop when **Goal**, **Requirements**, **Behaviour**, and **Acceptance criteria** are specific enough to implement without guessing and **Open questions** is omitted, or when the user says stop. When you stop because the spec is ready, tell the user the next step is **spec-review**.

## Writing the spec

Keep the spec-init headings and fill them in. Do not add or rename headings.

- **Goal** — what problem this solves, and for whom
- **Requirements** — numbered **R1**, **R2**, … as "The system shall …". Replace every placeholder bullet.
- **Behaviour** — important workflows, edge cases, and error handling
- **Constraints** — technical, security, accessibility, or compatibility constraints. Omit the whole section when there are none.
- **Acceptance criteria** — numbered **AC1**, **AC2**, … as Given/when/then. Replace every placeholder bullet.
- **Open questions** — unresolved decisions. Omit the whole section when none remain.

Keep the user's title. Fix incomplete sentences and typos while updating.

Fold in matching `AGENTS.md` / codebase conventions. If the spec would contradict them, ask before choosing a side.

Match the specificity of `specs/001-component-structure/spec.md` (files, APIs, behaviour, acceptance criteria) — not that file's heading layout.

Keep the spec concise. Prefer bullets over prose. Do not add rationale, alternatives considered, or implementation steps unless the user asked for them.
