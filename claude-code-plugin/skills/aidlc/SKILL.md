---
name: aidlc
description: >-
  AI-DLC adaptive software development workflow.
  Guides through inception, construction, and operations
  phases with structured requirements, design, and code generation.
argument-hint: "[phase-name | requirement description]"
disable-model-invocation: true
allowed-tools: Read, Write, Edit, Grep, Glob, Bash, AskUserQuestion, Task
---

# AI-DLC Skill Entry Point

## Argument Parsing

Analyze `$ARGUMENTS` to determine the entry mode:

1. **Phase-specific entry** — If `$ARGUMENTS` exactly matches one of:
   - `inception` → Jump directly to the Inception phase
   - `construction` → Jump directly to the Construction phase
   - `operations` → Jump directly to the Operations phase

2. **Requirement text** — If `$ARGUMENTS` is non-empty but not a phase name, treat it as an initial requirement description. Begin the full workflow using this text as the user's initial request.

3. **No arguments** — If `$ARGUMENTS` is empty, start the full workflow from the beginning with a welcome message and requirements gathering.

## Startup Sequence

### Step 1: Display Welcome Message
Read and display the welcome message from `rules/common/welcome-message.md`.
Only display this once at the start of a new workflow.

### Step 2: Initial Requirements Gathering
Use the `AskUserQuestion` tool to gather initial project context with 1-3 questions:

- **Project Type**: Is this a new project (greenfield) or an existing codebase (brownfield)?
- **Request Summary**: What do you want to build or change? (if not already provided via `$ARGUMENTS`)
- **Scope Estimate**: How would you describe the scope — single component, multiple components, or system-wide?

If `$ARGUMENTS` already contains a requirement description, skip the "Request Summary" question and use the provided text.

### Step 3: Load Core Workflow
Read `rules/core-workflow.md` to understand the full adaptive workflow.

### Step 4: Execute Workflow
Follow the core workflow rules, loading phase-specific rules from:
- `rules/common/` — Cross-cutting rules (always load at start)
- `rules/inception/` — Inception phase rules
- `rules/construction/` — Construction phase rules
- `rules/operations/` — Operations phase rules

Load rules dynamically as each phase/stage begins, following the instructions in `core-workflow.md`.

## Rule Loading Convention

All rule file paths in the workflow are relative to this skill's `rules/` directory.
When `core-workflow.md` says to "load `inception/workspace-detection.md`", read the file at `rules/inception/workspace-detection.md`.

## Question Handling

This plugin supports two methods for asking user questions:

1. **AskUserQuestion tool** — For quick, simple questions (1-3 questions). Preferred for initial requirements gathering and simple confirmations.
2. **Question files (.md)** — For comprehensive question sets (4+ questions). Follow the format in `rules/common/question-format-guide.md`.

Choose the appropriate method based on the number and complexity of questions needed.

## Claude Code Tool Mapping

When executing AI-DLC operations, use these Claude Code tools:

| AI-DLC Operation | Primary Tool | Notes |
|---|---|---|
| Read artifacts / rule files | Read | Load .md files by absolute path |
| Create new files | Write | Greenfield code, new documentation |
| Modify existing files | Edit | Brownfield code changes, state updates |
| Scan workspace structure | Glob | Patterns like `**/*.ts`, `**/package.json` |
| Search file contents | Grep | Find code patterns, verify implementations |
| Run build / test commands | Bash | Execute shell commands for build and test |
| Ask user questions (1-3) | AskUserQuestion | Quick confirmations and simple choices |
| Delegate parallel work | Task | Independent sub-tasks during construction |

### Key Principles

- **Read before Edit**: Always read a file before modifying it with Edit.
- **Glob before Read**: Use Glob to discover file paths before reading them.
- **Grep for verification**: After code generation, use Grep to verify implementations.
- **Bash for execution only**: Use Bash for build/test commands, not for file operations.
