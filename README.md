# Spec-driven development skills

Agent skills that take one feature from a draft specification to implementation. The specification states what the system shall do. A later plan states how to build it. Implementation follows that plan.

Spec format and intended to be aligned with a simplified version of the specification against the requirement-quality rules in [ISO/IEC/IEEE 29148:2018](https://www.iso.org/standard/72089.html). They do not implement the standard’s full requirements-engineering life cycle.

## Overview of standards and methodology

### Spec-driven development

[Spec-driven development](https://arxiv.org/html/2602.00180v1) treats the specification as the source of intent. Code is written or checked against that specification. Each phase produces an artifact that constrains the next, and a person reviews the artifact before the next phase starts.

1. **Specify.** What should the software do? Behaviour, requirements, and acceptance criteria, without prescribing the implementation.
2. **Plan.** How should it be built? Architecture, interfaces, and technical constraints.
3. **Implement.** Build it in small tasks, against the specification and the plan.
4. **Validate.** Check that the result meets the specification.

Teams use three levels of rigor:

- **Spec-first.** Write the specification before code. After implementation it may go stale. This is the entry point.
- **Spec-anchored.** Keep the specification in step with the code for the life of the system.
- **Spec-as-source.** People edit the specification, and code is regenerated from it.

This repository is spec-first. The specification is written and reviewed before `plan.md`, `task.md`, or code. The skills do not update the specification after implementation, and they do not treat it as generated source.

### ISO/IEC/IEEE 29148

[ISO/IEC/IEEE 29148:2018](https://www.iso.org/standard/72089.html), *Systems and software engineering — Life cycle processes — Requirements engineering* (second edition, 2018-11, confirmed 2024), covers requirements processes and products through the life cycle. It describes well-formed textual requirements, guides the requirements-related processes in ISO/IEC/IEEE 15288 and ISO/IEC/IEEE 12207, and specifies the information items those processes produce and what those items contain.

**spec-review** applies three parts of clause 5:

- **5.2.5, individual requirements.** Necessary, appropriate, unambiguous, complete, singular, feasible, verifiable, correct, and conforming.
- **5.2.6, a set of requirements.** Complete, consistent, feasible, comprehensible, and able to be validated.
- **5.2.7, language.** `shall` for a mandatory requirement on the system, `should` for a non-mandatory goal, `may` for permission, and `will` for a fact about another party.

`spec.md` records one feature: goal, requirements, behaviour, constraints, and acceptance criteria. 

## Skills

Each skill sets `disable-model-invocation: true`. The agent loads a skill when you say its name or @-mention it. Run them in this order on one feature.

### spec-init

Creates `specs/<NNN>-<slug>/spec.md` from [the template](spec-init/template.md). `<NNN>` is the next three-digit prefix under `specs/` (`001` if none exist). It does not fill gaps. The slug is short kebab-case. The file keeps the template headings. Only facts you already stated replace placeholders. Constraints and Open questions are included only when you already gave content for them. It does not grill, plan, or implement. Next step: **spec-grill-me**.

### spec-grill-me

Turns the draft into a specification an implementer can follow. It reads the spec and the project’s conventions, then asks 3–6 questions per round about scope, files, API, UX, constraints, or acceptance. After each round it updates `spec.md` in place. It stops when Goal, Requirements, Behaviour, and Acceptance criteria are specific enough to implement and Open questions is omitted, or when you say stop. Requirements use **R1**, **R2**, … as “The system shall …”. Acceptance criteria use **AC1**, **AC2**, … as Given/when/then. Next step: **spec-review**.

### spec-review

Reviews `spec.md` against clauses 5.2.5, 5.2.6, and 5.2.7, and against trace: every **R** has at least one **AC**, and every **AC** cites the **R** it verifies. It writes `review.md` beside the spec. `Result` is `Pass` or `Findings open`. Open findings are **F1**, **F2**, …, each naming the requirement (or the set), the failed characteristic, and the fix. A template prompt or an Open questions section still present cannot pass. Resolve findings with **spec-grill-me**, then run **spec-review** again. On pass, next step: **spec-plan**.

### spec-plan

Writes `plan.md` and `task.md` in the same folder. It stops, and writes neither file, when Goal, Requirements, Behaviour, or Acceptance criteria are still a template or too vague to implement, when Open questions is still present, or when `review.md` is missing, is not `Pass`, or is older than `spec.md`. It asks only implementation questions: files, APIs, types, order, and how. `plan.md` covers technical context, target structure, implementation detail, order, and out of scope. `task.md` is grouped unchecked tasks, including a Verify section. It does not edit the spec or implement it.

### spec-execute

Implements the remaining unchecked items in `task.md`, in order, including Verify. It marks each item done only after the work is done. It does not rewrite the spec, plan, or tasks, and it does not commit. If `task.md` is missing or too vague, it stops and points you to **spec-plan**.

## Install and use

Install the skills with the Skills CLI.

In one project:

```bash
npx skills add danpaul/agent_skills_sdd
```

For every project:

```bash
npx skills add danpaul/agent_skills_sdd -g
```

In that project’s agent chat, name the skill or @-mention it. For a new feature, start with **spec-init**. For an existing spec, point **spec-grill-me**, **spec-review**, and **spec-plan** at `specs/<NNN>-<slug>/spec.md`. Point **spec-execute** at that folder’s `task.md`. If the topic or the spec path is unclear, the skill asks before it writes anything.
