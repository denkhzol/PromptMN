# PromptMN Language Specification

**Version:** 1.0.0
**Author:** Enkhzol Dovdon
**License:** MIT
**Copyright:** © 2026 Enkhzol Dovdon

PromptMN is a pseudo-prompting language (a domain-specific language, DSL) designed
to provide **typed, semantic directives** for AI prompt interpretation and execution.

PromptMN defines a set of reserved keywords that function as interpreter directives.
These directives govern how an AI model interprets, organises, and executes prompt
instructions.

PromptMN annotates prompts using keywords written inline with prose, prefixed with
`%`, and **resolved semantically before execution**. This allows authors to compose
prompts in any reading order without affecting interpretation.

The delimiter pair `∞ … ∞` marks the program boundary and encloses the language's
complete token set. Within this boundary, tokens are separated by commas (`,`), and
every listed token carries a distinct meaning that the interpreter must correctly
recognise and apply.

Keywords without an explicit format use the default forms `%keyword <text>;` or
`%keyword {statements;}`.

## Token Set

```
∞;,{,},\n,%role,%intro,%goal,%techs,%aware,%risk,%problem,%example,%note,%label,
%domain,%req,%reqfunc,%reqnonfunc,%newreq,%should,%could,%optional,%rule,%mustnot,
%plan,%number,%showplan,%trace,%if,%else,%repeat,%continue,%break,%goto,%var,
%method,%return,%in,%data,%ignore,%out,%visualize,%diagram,%add,%del,%update,
%addition,%newconcept,%format,%meta∞
```

The keywords are organised into seven contextual clusters. Cluster 1 applies to the
entire prompt as its foundation.

---

## Cluster 1 — Lexical Syntax (Foundational)

| Token | Meaning |
| --- | --- |
| `%keyword` | Generic notation for a unique command that provides additional instructional information. Keywords may appear multiple times and in any position. The interpreter logically reorders keywords, resolving them by **semantics rather than source order**, before executing. |
| `;` | Semicolon completes an expression statement. |
| `{ … }` | Curly braces define a **block scope**, grouping a sequence of statements that belong to the command immediately preceding them. Format: `%keyword {statement; statement; ...}` |
| `\n` | Newline indicates a line break. After `\n`, a new term or expression begins. |
| `<condition>` | Represents a condition evaluated as true/false or satisfied/not satisfied. May be a simple declaration or a logical expression. |

> Angle-bracket tokens such as `<condition>`, `<name>`, or `<expression>` are
> **meta-syntactic placeholders** and are not themselves registered keywords.

---

## Cluster 2 — Context and Intent

| Keyword | Meaning | Format / Example |
| --- | --- | --- |
| `%role` | Assigns one or more personas to the AI. All subsequent directives are processed through this persona lens. | `%role <role>;` or `%role {role1; role2;}` — e.g. `%role senior software architect;` |
| `%intro` | Background information or contextual overview of the problem, domain, or scope. | |
| `%goal` | Intended purpose or success criteria; what a good outcome looks like (distinct from `%problem`). | `%goal <text>;` or `%goal {statements;}` |
| `%techs` | Declares preferred technology stack(s); recommendations must favour these unless a constraint prevents it. | |
| `%aware` | Information, assumptions, risks, or conditions to be aware of when applying requirements. | |
| `%risk` | Flags a potential event that may negatively affect the outcome. | |
| `%problem` | States the situation or need — what is wrong or missing before choosing a solution. | |
| `%example` | A supporting illustration (scenario or input/output) clarifying meaning; not a replacement for requirements. | |
| `%note` | A brief supporting point to clarify or highlight information. | |
| `%label` | A short name or tag to identify, categorize, or describe an item quickly. | |
| `%domain` | Specifies the applicable domain and supplies its terminology, conventions, and background; scopes the model's frame of reference for all later directives. | |

---

## Cluster 3 — Requirements & Governance

| Keyword | Meaning |
| --- | --- |
| `%req` | A testable requirement of what must be done or satisfied. |
| `%reqfunc` | Functional requirement — a behavior the system must perform (what it does). |
| `%reqnonfunc` | Non-functional requirement — a quality attribute or constraint (performance, security, reliability). |
| `%newreq` | Introduces a new requirement; on conflict it **overrides** existing requirement(s). A typed override scoped to requirements (vs `%update`'s generic edit). |
| `%should` | Should-have: important but not vital. |
| `%could` | Could-have: nice to have; deliver only if time/resources remain. |
| `%optional` | Item(s) to incorporate unless doing so conflicts with another requirement or constraint. |
| `%rule` | A condition-based policy defining what is allowed, required, or triggered in given situations. |
| `%mustnot` | A hard constraint forbidding behaviors, technologies, or conditions. Format: `%mustnot {text1; text2;}` — e.g. `%mustnot do not hallucinate` |

---

## Cluster 4 — Planning & Orchestration

| Keyword | Meaning | Format / Example |
| --- | --- | --- |
| `%plan` | States the intended outcome or high-level strategy rather than the program itself. | |
| `%<number>` | A sequentially ordered step; the integer indicates execution position. Steps may appear anywhere; the interpreter collects and sequences them ascending before execution. | `%1 load dataset; %2 preprocess features; %3 train model; %4 evaluate metrics;` |
| `%showplan` | The AI must explicitly present its execution plan before performing any actions. | |
| `%trace` | The AI must emit an execution trace / reasoning summary alongside its output (delivered via `%out`); complements `%showplan`. | |

---

## Cluster 5 — Control-Flow & Computation

| Keyword | Meaning | Format / Example |
| --- | --- | --- |
| `%if` | If `<condition>` is true, execute the statement/block; if false, skip or perform an alternative. | `%if <login is successful> {navigate to the Home page;}` |
| `%else` | Defines an alternative action for the preceding `%if`. | `%else {show the error message and offer sign-up;}` |
| `%repeat` | While `<condition>` is true, execute the statement/block repeatedly until it becomes false. | `%repeat <there is no obstacle> the player should continue its movement;` |
| `%continue` | Skips the rest of the current `%repeat` iteration and re-evaluates the condition. | `%continue;` |
| `%break` | Terminates the enclosing `%repeat` loop immediately and resumes after the loop. | `%break;` |
| `%goto` | Continues processing at a labeled statement. Labels are declared inline as `%jumplabel-<N>:`. | `%jumplabel-1: retry the request; %if <failed> {%goto %jumplabel-1;}` |
| `%var` | Declares a mutable named value; also a placeholder substituted by its value wherever `%<name>` appears in text. | `%var %customer = "Alice"; %out "Dear %customer, your order is confirmed.";` |
| `%method` | Defines a named unit of behavior. | `%method %greet(%user) {%out "Hello, %user";}` |
| `%return` | Ends the current `%method`, optionally returning a value/expression; without a value, exits with no result. | `%method %max(%a, %b) {%if <%a > %b> {%return %a;} %return %b;}` |

---

## Cluster 6 — Data Interface

| Keyword | Meaning | Format / Example |
| --- | --- | --- |
| `%in` | Provides input data (files, structured, or unstructured) for an AI model or process. | |
| `%data` | Declares a named, structured collection (record, table, list, or key-value set) treated as typed, addressable data rather than free text. Referenced by `%in`, `%out`, `%method`, or steps. | `%data %user {name: "Frank"; role: "admin"; active: true;}` or `%data %scores [90; 85; 92;]` |
| `%ignore` | Excludes and disregards specified file(s), directories (and all contents), or context elements. Never read or factored in. | `%ignore {draft/; example.md;}` |
| `%out` | Declares the observable output. | |
| `%visualize` | Renders the subject as a visual artifact rather than (or in addition to) prose. A specialization of `%out` that constrains the medium. | `%visualize <user login flow> {format: mermaid; type: sequenceDiagram;}` |
| `%diagram` | Shorthand for `%visualize` using Mermaid as the default format. | `%diagram <component architecture> {type: flowchart;}` |

---

## Cluster 7 — Meta & Lifecycle Operations

| Keyword | Meaning | Format / Example |
| --- | --- | --- |
| `%add` | Introduces a new element into the current context. | |
| `%del` | Deletes an existing element from the current context. | |
| `%update` | Modifies an existing element in the current context. | |
| `%addition` | Provides additional information, written using prompt syntax. | |
| `%newconcept` | Introduces a new keyword together with its explanation; overrides on conflict. Introduces a language element (vs `%add`'s content element). | `%newconcept %<keyword> {explanation;}` |
| `%format` | Declares a default rendering format for `%out`/`%visualize` in scope (markdown, json, yaml, plain text). | `%format markdown;` |
| `%meta` | Declares metadata about the PromptMN program itself (author, version, target, audience, locale). Descriptive by default; agents may use it to gate or adapt execution. | `%meta {author: Enkhzol Dovdon; version: 1.0.0; license: MIT; target: any-LLM;}` |
