# PromptMN — Concise Reference

> PromptMN v1.0.0 — a pseudo-prompting DSL giving typed, semantic directives for AI
> prompt interpretation/execution.
>
> Author: Enkhzol Dovdon · License: MIT · © 2026 Enkhzol Dovdon

## Syntax at a glance

- Keywords are inline directives prefixed with `%`, **resolved semantically** (not by
  source order). Authors may write keywords in any order/position; the interpreter
  reorders by meaning before executing.
- Default forms: `%keyword <text>;` or `%keyword {statement; statement; ...}`.
- Program boundary: `∞ ... ∞` encloses the full token set; tokens are comma-separated;
  each token has a distinct meaning the interpreter must apply.

```
∞;,{,},\n,%role,%intro,%goal,%techs,%aware,%risk,%problem,%example,%note,%label,
%domain,%req,%reqfunc,%reqnonfunc,%newreq,%should,%could,%optional,%rule,%mustnot,
%plan,%number,%showplan,%trace,%if,%else,%repeat,%continue,%break,%goto,%var,
%method,%return,%in,%data,%ignore,%out,%visualize,%diagram,%add,%del,%update,
%addition,%newconcept,%format,%meta∞
```

Keywords are grouped into 7 clusters. Cluster 1 underlies the whole prompt.

## Cluster 1 — Lexical Syntax (foundational)

- `%keyword` — generic notation for a unique command adding instructional info; resolved by semantics, not order.
- `;` — ends a statement.
- `{ ... }` — block scope grouping statements for the preceding command.
- `\n` — newline; a new term/expression begins after it.
- `<condition>` — something evaluable as true/false or satisfied/not.
- Angle-bracket tokens (`<condition>`, `<name>`, `<expression>`) are meta-syntactic placeholders, **not** keywords.

## Cluster 2 — Context and Intent

- `%role` — assigns persona(s); later directives interpreted through this lens.
- `%intro` — background/contextual overview.
- `%goal` — intended purpose/success criteria (distinct from `%problem`).
- `%techs` — preferred tech stack(s).
- `%aware` — info/assumptions/risks/conditions to keep in mind.
- `%risk` — a potential harmful event to account for.
- `%problem` — what is wrong or missing, before choosing a solution.
- `%example` — supporting illustration; not a substitute for requirements.
- `%note` — brief clarifying point.
- `%label` — short name/tag to identify or categorize.
- `%domain` — sets the domain and supplies its terminology/conventions.

## Cluster 3 — Requirements & Governance

- `%req` — a testable requirement.
- `%reqfunc` — functional requirement (a behavior the system must perform).
- `%reqnonfunc` — non-functional requirement (quality/constraint).
- `%newreq` — new requirement; overrides on conflict (typed override).
- `%should` — Should-have: important but not vital.
- `%could` — Could-have: nice to have.
- `%optional` — incorporate unless it conflicts with another requirement.
- `%rule` — condition-based policy.
- `%mustnot` — hard constraint forbidding behaviors/technologies/conditions.

## Cluster 4 — Planning & Orchestration

- `%plan` — intended outcome/high-level strategy.
- `%<number>` — a sequentially ordered step (e.g. `%1`, `%2`); sequenced ascending before execution.
- `%showplan` — present the execution plan before acting.
- `%trace` — emit an execution trace/reasoning summary with output (via `%out`).

## Cluster 5 — Control-Flow & Computation

- `%if <condition>` — execute block if true, else skip/alternative.
- `%else` — alternative for the preceding `%if`.
- `%repeat <condition>` — loop while true.
- `%continue` — skip to next loop iteration.
- `%break` — exit the enclosing loop.
- `%goto` — jump to a `%jumplabel-<N>:` target.
- `%var` — declares a mutable named value / substitution placeholder.
- `%method` — defines a named unit of behavior.
- `%return` — ends a `%method`, optionally returning a value.

## Cluster 6 — Data Interface

- `%in` — provides input data.
- `%data` — declares a named structured collection (record/table/list/key-value).
- `%ignore` — exclude file(s)/directories/context elements entirely.
- `%out` — declares the observable output.
- `%visualize` — render the subject as a visual artifact (specialization of `%out`).
- `%diagram` — shorthand for `%visualize` with a Mermaid default.

## Cluster 7 — Meta & Lifecycle Operations

- `%add` — introduce a new element into context.
- `%del` — delete an existing element.
- `%update` — modify an existing element.
- `%addition` — provide extra information in prompt syntax.
- `%newconcept` — introduce a new keyword + explanation; overrides on conflict.
- `%format` — default rendering format for `%out`/`%visualize`.
- `%meta` — metadata about the PromptMN program itself.
