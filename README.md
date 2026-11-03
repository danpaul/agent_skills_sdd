# Spec-driven development skills

Agent skills that take one feature from a draft specification to implementation. The specification states what the feature does. A later plan states how to build it. Implementation follows that plan.

A spec has four sections: **Goal**, **Requirements**, **Acceptance Criteria**, and **Out of Scope**. Requirements, acceptance criteria, and out-of-scope items are plain bullets. The spec has no title and no requirement ids.

## Overview

### Spec-driven development

[Spec-driven development](https://arxiv.org/html/2602.00180v1) treats the specification as the source of intent. Code is written or checked against that specification. Each phase produces an artifact that constrains the next, and a person reviews the artifact before the next phase starts.

1. **Specify.** What should the feature do? Goal, requirements, acceptance criteria, and what is out of scope.
2. **Plan.** How should it be built? Architecture, interfaces, and technical constraints.
3. **Implement.** Build it in small tasks, against the specification and the plan.
4. **Validate.** Check that the result meets the specification.

Teams use three levels of rigor:

- **Spec-first.** Write the specification before code. After implementation it may go stale. This is the entry point.
- **Spec-anchored.** Keep the specification in step with the code for the life of the system.
- **Spec-as-source.** People edit the specification, and code is regenerated from it.

This repository is spec-first. The specification is written and clarified before `plan.md`, `task.md`, or code. The skills do not update the specification after implementation, and they do not treat it as generated source.

## Skills

Each skill sets `disable-model-invocation: true`. The agent loads a skill when you say its name or @-mention it. Run **spec-init**, **spec-grill-me**, **spec-plan**, and **spec-execute** in that order on one feature.

### spec-init

Creates `specs/<NNN>-<slug>/spec.md` from [the template](spec-init/template.md). `<NNN>` is the next three-digit prefix under `specs/` (`001` if none exist). It does not fill gaps. The slug is short kebab-case. The file keeps the template headings and does not add a title. Only facts you already stated replace placeholders. It does not grill, plan, or implement. Next step: **spec-grill-me**.

### spec-grill-me

Turns the draft into a specification an implementer can follow. It reads the spec and the project’s conventions, then asks 3–6 questions per round about scope, files, API, UX, constraints, or acceptance. After each round it updates `spec.md` in place. Requirements, acceptance criteria, and out-of-scope items are plain bullets. It stops when Goal, Requirements, and Acceptance Criteria are specific enough to implement, or when you say stop. A placeholder Out of Scope section does not block that. It does not write a review. Next step: **spec-plan**.

### spec-plan

Writes `plan.md` and `task.md` in the same folder. It stops, and writes neither file, when Goal, Requirements, or Acceptance Criteria are still a template or too vague to implement. It asks only implementation questions: files, APIs, types, order, and how. `plan.md` covers technical context, target structure, implementation detail, order, and out of scope. `task.md` is grouped unchecked tasks, including a Verify section. It does not edit the spec or implement it.

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

In that project’s agent chat, name the skill or @-mention it. For a new feature, start with **spec-init**. For an existing spec, point **spec-grill-me** and **spec-plan** at `specs/<NNN>-<slug>/spec.md`. Point **spec-execute** at that folder’s `task.md`. If the topic or the spec path is unclear, the skill asks before it writes anything.
