# Ubiquitous Language

> Domain: **GitHub Copilot customization toolkit** — files, conventions, and patterns for shaping Copilot's behavior inside a VS Code workspace.

---

## Core artifacts

| Term | Definition | Aliases to avoid |
| --- | --- | --- |
| **Agent** | A named Copilot mode defined by a `.agent.md` file; encapsulates a system prompt, a tool set, and a model selection for a specific workflow | Bot, assistant, custom mode |
| **Instruction** | A Markdown file that declares rules guiding one or more agents; always-on or scoped to file types via `applyTo` | Rule file, prompt rule, guideline |
| **Skill** | A loadable capability file (`SKILL.md`) that an agent reads on demand when a described trigger condition matches the user's request | Plugin, module, add-on |
| **Prompt** | A slash-command-invocable `.prompt.md` file for lightweight, one-shot workflows that do not require a persistent agent session | Template, script, shortcut |

---

## Agent roles

| Term | Definition | Aliases to avoid |
| --- | --- | --- |
| **Orchestrator** | The top-level agent that decomposes a task, delegates subtasks to subagents, and merges their outputs | Coordinator, main agent, parent |
| **Subagent** | A worker agent spawned by an orchestrator to handle one atomic, well-scoped subtask | Child agent, helper, worker |
| **Domain expert** | The role in a glossary or grill-me dialogue that holds authoritative knowledge about business concepts; not necessarily a person | Business analyst, stakeholder |

---

## Configuration

| Term | Definition | Aliases to avoid |
| --- | --- | --- |
| **Frontmatter** | The YAML block delimited by `---` at the top of a customization file; declares metadata such as `applyTo`, `description`, `model`, and `tools` | Header, YAML header, metadata block |
| **applyTo** | The frontmatter field that scopes an Instruction to a glob pattern of file paths; omitting it makes the Instruction always-on | Scope, target, filter |
| **Trigger** | The condition — expressed in a Skill's `description` frontmatter — that causes an agent to load and apply that Skill | Activation condition, use-when clause |
| **Tool** | A discrete capability (file read, terminal execution, web fetch, etc.) that an agent may be permitted or restricted to call | Function, action, command |

---

## Workflows

| Term | Definition | Aliases to avoid |
| --- | --- | --- |
| **Grill session** | An iterative interview workflow in which Copilot probes every branch of a plan until all decisions are resolved; terminates with a Decision Summary | Review, planning session |
| **Decision Summary** | The structured output of a grill session: each decision explored, its resolution, dependencies, accepted trade-offs, and deferred items | Summary, outcome doc |
| **Blueprint** | A structured, atomic implementation plan produced by the plan-blueprint skill, including risk levels, effort estimates, a dependency diagram, and a summary table | Spec, roadmap, design doc |
| **Story point (SP)** | The Fibonacci-scale unit used to estimate effort in a Blueprint | Points, complexity score, estimate |
| **Keep-alive** | The convention of ending each task step with a check-in question so the user can steer or close the session | Follow-up, ping, continuation prompt |

---

## Relationships

- An **Agent** may reference zero or more **Skills**; Skills are loaded only when their **Trigger** matches the current request.
- An **Instruction** is injected into the context of every agent whose file path matches the **applyTo** glob; an Agent without a matching Instruction operates without that rule.
- An **Orchestrator** spawns one or more **Subagents**, each with a single atomic task; the Orchestrator owns the final synthesis.
- A **Prompt** is a degenerate Agent with no persistent session; it runs once, produces output, and exits.
- A **Blueprint** is the primary artifact of the plan-blueprint **Skill**; a **Grill session** is how a **Blueprint** (or any plan) gets stress-tested before execution.

---

## Example dialogue

> **Dev:** "I want Copilot to enforce our C# naming conventions automatically — where does that live?"

> **Domain expert:** "That's an **Instruction**. Create a `.instructions.md` file under `.github/instructions/` and set `applyTo: "**/*.cs"` in its **Frontmatter**. Any agent working on a `.cs` file will receive those rules."

> **Dev:** "What if I want a full code-review workflow, not just naming rules?"

> **Domain expert:** "That's an **Agent**. Define its system prompt and permitted **Tools** in a `.agent.md` file. If the review logic is reusable across multiple agents, extract it into a **Skill** instead."

> **Dev:** "When would I use a **Prompt** versus an **Agent**?"

> **Domain expert:** "A **Prompt** is one-shot — invoke it, get output, done. An **Agent** maintains a workflow identity across an entire session. If you're generating a PR description, a **Prompt** is right. If you're running a full sprint planning session, you want an **Agent**, probably acting as an **Orchestrator**."

> **Dev:** "Before I build the agent, should I blueprint it first?"

> **Domain expert:** "Yes — run the plan-blueprint **Skill** to get a **Blueprint**. Then use a **Grill session** to stress-test every decision before writing any files."

---

## Flagged ambiguities

- **"agent"** is used in two senses: (1) a custom `.agent.md` file defining a Copilot mode, and (2) any Copilot instance acting at runtime (including the built-in assistant). Prefer **Agent** for the file-defined mode; use "Copilot instance" when referring to the runtime.
- **"prompt"** appears both as the noun for a `.prompt.md` slash-command file and generically for any text sent to the model. Reserve **Prompt** strictly for the file artifact; use "message" or "input" for the generic case.
- **"skill"** is sometimes used loosely to mean "a thing Copilot can do". Here it refers exclusively to a `SKILL.md` file loaded on demand — a named, file-backed capability, not a general ability.
