# j-token-workflow-kit

[한국어 README](README.ko.md)

TL;DR: `j-token-workflow-kit` is a Claude Code workflow plugin that turns rough requests into reviewable specs, code changes, and verification steps. It is designed for work that starts vague and needs to become concrete before implementation.

Current plugin version: `0.7.0`

## Why This Exists

Claude can implement quickly, but unclear requests can still produce unclear changes. This kit adds a lightweight workflow around the conversation:

- clarify the requirement through dialogue
- turn the clarified requirement into a document
- use review comments to refine the document
- implement from the reviewed document
- verify the result after implementation

## Workflow

```mermaid
flowchart LR
    A["Work request"] --> B["Clarify requirements through conversation"]
    B --> C["Document the agreed requirements"]
    C --> D["Review and revise with comments"]
    D --> E["Implement"]
    E --> F["Verify after implementation"]
```

## How To Use

To avoid confusion with Claude Code's built-in `Workflow` tool, invoke this kit explicitly with the slash command:

```text
/j-token-workflow
```

Claude should first ask only the questions needed to reduce ambiguity. After the requirement is clear, ask it to write the document:

```text
Document this as an implementation spec.
```

Review the document with review comments. Use them to fix missing details, risky assumptions, or unclear acceptance criteria.

When the document is ready, ask Claude to implement it:

```text
Implement this.
```

After implementation, Claude should verify the result and report what was checked.

## Included Skills

| Skill | Purpose |
|---|---|
| `j-token-workflow` | Explicit entry point (`/j-token-workflow`) for this kit's workflow, avoiding confusion with Claude Code's built-in `Workflow` tool. |
| `requirements-to-spec` | Turns rough requirements into a concrete spec and implementation document. |
| `prd-writer` | Drafts technical PRDs for products, SDKs, CLIs, runtimes, and developer tools. |
| `technical-spec-writer` | Turns technical design notes into implementation specs with APIs, protocols, boundaries, and tests. |
| `audit-technical-spec` | Audits a technical spec against the repo and official sources with a fresh no-context agent before implementation. |
| `start-implementation` | Gates on approval and a passed audit, then implements via plan mode with mandatory independent pre/post reviews. |
| `orchestrate-subagents` | Delegates only gated, independent tasks to role-based subagents with file ownership and verification contracts. |
| `prototype-design` | Builds and iterates a clickable web prototype before the design or flow is finalized. |
| `bug-report-to-fix` | Captures bug details first, then moves into debugging and fixing after approval. |
| `figma-flow-to-implementation` | Converts Figma links, screenshots, or UI references into a screen flow and implementation spec. |
| `workflow-composer` | Combines multiple workflows when a request mixes requirements, bugs, and UI work. |
| `cognitive-writing` | Keeps documents easy to review by reducing cognitive load. |
| `branch-rule` | Defines branch naming rules. |
| `commit-rule` | Defines commit message rules. |
| `git-push-safety` | Prevents accidental pushes to the wrong branch. |
| `pr-rule` | Defines pull request writing rules. |

## Recommended Prompts

```text
Use the workflow to clarify this requirement before implementation.
```

```text
Document the agreed requirement as a spec.
```

```text
Draft this product idea as a PRD.
```

```text
Turn this runtime design into a technical implementation spec.
```

```text
Review this spec and leave comments for anything unclear.
```

```text
Apply the review comments, then implement the spec.
```

```text
Verify the implementation and summarize the result.
```

## Repository Layout

```text
.claude-plugin/marketplace.json
plugins/claude-workflow/.claude-plugin/plugin.json
plugins/claude-workflow/skills/
plugins/claude-workflow/references/
```

## Installation

Add this repository as a marketplace in Claude Code, then install the plugin.

```text
/plugin marketplace add j-token/j-token-claude-workflow-kit
/plugin install claude-workflow@j-token-workflow-kit
```

## License

MIT
