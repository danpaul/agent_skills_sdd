---
name: spec-review
description: >-
  Reviews a feature spec against ISO/IEC/IEEE 29148 requirement
  characteristics and language rules, and writes a colocated review.md. Does
  not edit the spec, plan, or implement. Use when the user says "spec-review",
  @-mentions this skill, or asks to review a spec for requirements quality.
disable-model-invocation: true
---

# Spec review

Review one `spec.md` against ISO/IEC/IEEE 29148 and write `review.md` beside it.

Do not edit `spec.md`, `plan.md`, or `task.md`. Do not implement. Do not invent requirements.

If the spec path is unclear, ask instead of guessing.

## When invoked

1. Identify the spec (`@` mention, path, or `specs/<id>/spec.md`). If none is clear, ask which spec to review.
2. Read `spec.md`. Do not read `plan.md` or `task.md` to soften a finding.
3. Check every rule below against the current spec.
4. Replace `review.md` in that folder. Do not append. Do not ask before replacing.
5. Summarize the result in chat. If findings are open, tell the user to resolve them with **spec-grill-me**, then run **spec-review** again. On Pass, the next step is **spec-plan**.

## Writing review.md

```markdown
# Review

Source: ./spec.md
Result: Findings open

## Findings

- **F1** (R2, Unambiguous): "fast" has no bound. State a measurable limit.
```

- `Result` is `Pass` or `Findings open`.
- On Pass, omit **Findings**.
- Otherwise list **F1**, **F2**, … Each finding names the requirement id or `set`, the failed characteristic, and the concrete fix.
- A characteristic failure is a finding. Do not file style nits. Pass only when the findings list is empty.
- **Open questions** still present, or a template prompt still in place, is a finding. That review cannot Pass.

## Individual requirements

Apply to each **R** and the **AC** that verifies it (ISO/IEC/IEEE 29148:2018, 5.2.5).

- **Necessary** — essential capability, characteristic, constraint, or quality. Flag duplicates.
- **Appropriate** — obligation at requirement level. Flag file paths, libraries, and internal structure in **Requirements** unless **Constraints** records an interface the product cannot change.
- **Unambiguous** — one reading. Flag unbounded words (fast, easy, robust, user-friendly, intuitive, flexible, efficient), "and/or", "etc.", "TBD", and unclear pronouns.
- **Complete** — the statement includes the condition when the obligation depends on one.
- **Singular** — one "shall" or "should". Flag "and" that joins two obligations.
- **Feasible** — achievable under **Constraints**.
- **Verifiable** — observable fit. Flag an **R** with no **AC**, and an **AC** that restates the requirement without an observable outcome.
- **Correct** — agrees with **Goal**.
- **Conforming** — "The system shall/should …", ids **R1** / **AC1**, and `AC1 (verifies R1):`.

## Set of requirements

Apply to the spec as a whole (5.2.6). Name these findings `(set, …)`.

- **Complete** — **Goal**, behaviour, errors, and constraints cover the need.
- **Consistent** — no conflicts among requirements, behaviour, constraints, and acceptance criteria.
- **Feasible** — the set can be satisfied together.
- **Comprehensible** — each obligation can be found. Flag an undefined domain term that changes the meaning.
- **Able to be validated** — **Open questions** is omitted, and the requirements address the need in **Goal**.

## Language

Apply 5.2.7.

- **shall** — mandatory requirement on the system.
- **should** — non-mandatory goal.
- **may** — permission.
- **will** — a fact about another party, not an obligation on the system. Flag **will** used as a system obligation.

## Trace

- Every **R** has at least one **AC**.
- Every **AC** cites the **R** it verifies.
- Do not require a rationale on every requirement. Flag a missing reason only when a constraint or quality requirement would be opaque without it.
