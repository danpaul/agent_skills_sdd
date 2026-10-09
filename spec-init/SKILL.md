---
name: spec-init
description: >-
  Creates a new spec at specs/NNN-kebab-slug/spec.md from the template in this
  skill, using the next sequential 3-digit prefix. Does not grill, plan, or
  implement. Use when the user says "spec-init", @-mentions this skill, or asks
  to start or initialize a new spec.
disable-model-invocation: true
---

# Spec init

Create one new spec folder from `template.md` in this skill's directory.

Do not grill the spec. Do not create `plan.md` or `task.md`. Do not implement. Do not edit `AGENTS.md` or `README.md`.

If the topic or slug is unclear, ask instead of guessing.

## When invoked

1. Get the feature from the user (message, or a short description they already gave). If there is no topic, ask what the spec is for before creating anything.
2. Choose the folder name (below). If that folder already exists, or another `specs/*-<slug>/` uses the same slug, stop and ask. Do not overwrite.
3. Read `template.md` in the same directory as this skill. Write that file to `specs/<NNN>-<slug>/spec.md`. Do not invent a different outline.
4. Replace `[Feature name]` with a short title taken from the user's words. Do not add or rename headings.
5. Draft only what the user already said (below).
6. Tell the user the path and that the next step is **spec-grill-me** on that `spec.md`.

## Naming and location

- One new folder: `specs/<NNN>-<slug>/`.
- Only file: `spec.md` in that folder.
- `<NNN>` is the next sequential prefix: among directories directly under `specs/` whose names match `NNN-…` (`NNN` is three digits), take the highest prefix, add 1, and zero-pad to 3 digits (`009` → `010`). If none exist, use `001`. Do not fill gaps.
- `<slug>` is kebab-case: lowercase `a-z`, digits, and single hyphens. Short (a few words). If the user gave a slug, normalize that. Otherwise derive it from the title.
- Do not put the number in the slug.

## Template

Read `template.md` next to this skill and copy it to `spec.md`. That file is the only outline. Do not add or rename headings.

## Rough draft

Under a heading, replace the template prompt or placeholder bullets only with facts the user already stated that belong there. Do not invent requirements, exclusions, or alternatives.

- **Goal**, **Requirements**, **Behaviour**, **Acceptance criteria**: if the user stated facts for that section, replace the prompt or placeholder bullets with those facts. Number real requirements **R1**, **R2**, … and real acceptance criteria **AC1**, **AC2**, … in the template's Given/when/then shape. If they said nothing for a section, leave that section exactly as the template wrote it.
- **Constraints** and **Open questions**: omit the whole section when the user stated nothing for it. Include it only when they already gave content for it.

## Example

User: "Initialize a spec for dark mode on Button." Highest existing prefix is `009`.

Create `specs/010-button-dark-mode/spec.md`:

```markdown
# Button dark mode

## Goal

- Dark mode styles for the Button atom.

## Requirements

- **R1:** The system shall ...
- **R2:** The system shall ...

## Behaviour

Describe important workflows, edge cases, and error handling.

## Acceptance criteria

- **AC1:** Given ..., when ..., then ...
- **AC2:** Given ..., when ..., then ...
```

**Constraints** and **Open questions** are omitted because the user stated nothing for them.
