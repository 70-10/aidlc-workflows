# AI-DLC Claude Code Plugin

AI-DLC (AI-Driven Development Life Cycle) workflow plugin for [Claude Code](https://claude.com/claude-code).

## What is AI-DLC?

AI-DLC is an adaptive software development workflow that intelligently guides you through the full development lifecycle — from requirements gathering through design, implementation, and testing. It adapts to your project's complexity, skipping unnecessary steps for simple changes while providing comprehensive coverage for complex projects.

## Installation

### Local Installation (for development/testing)

```bash
claude plugin install --local ./claude-code-plugin
```

### From GitHub

```bash
claude plugin marketplace add 70-10/aidlc-workflows
claude plugin install aidlc@aidlc-workflows
```

## Usage

### Start the full workflow
```
/aidlc
```

### Jump to a specific phase
```
/aidlc inception
/aidlc construction
/aidlc operations
```

### Start with a requirement description
```
/aidlc Build a REST API for user management with authentication
```

## Workflow Phases

1. **Inception** — Planning, requirements gathering, and architectural decisions
2. **Construction** — Detailed design, implementation, and testing
3. **Operations** — Deployment and monitoring (placeholder for future expansion)

## Prerequisites

- [Claude Code CLI](https://claude.com/claude-code) installed and authenticated
- A project directory (new or existing codebase)
- Node.js 18+ (required for Claude Code)

## Installation Verification

After installing the plugin, verify it is available:

```
claude skills list
```

You should see `aidlc` in the output. If not, re-run the installation command.

## Quick Start Example

**New project (greenfield)**:
```
/aidlc Build a REST API for task management with CRUD operations
```
The workflow will guide you through requirements analysis, design, code generation, and testing.

**Existing project (brownfield)**:
```
/aidlc Add user authentication to the existing API
```
The workflow will first analyze your codebase via reverse engineering, then proceed with requirements and implementation.

**Resume a previous session**:
```
/aidlc
```
If `aidlc-docs/aidlc-state.md` exists, the workflow automatically resumes from where you left off.

## Phase Details

### Inception
- **Workspace Detection**: Scans your project to determine greenfield vs brownfield
- **Reverse Engineering** (brownfield only): Analyzes existing code architecture
- **Requirements Analysis**: Structured requirements gathering with user questions
- **User Stories**: Generates user stories and personas from requirements
- **Classification**: Determines project complexity and appropriate workflow depth

### Construction
- **Application Design**: High-level architecture and component design
- **Unit Design**: Detailed design for each unit of work
- **NFR Implementation**: Non-functional requirements (security, performance)
- **Code Generation**: Step-by-step code generation with approval gates
- **Build and Test**: Build verification and comprehensive testing

### Operations
- **Deployment Planning**: Infrastructure and deployment configuration
- Future: Monitoring, scaling, and maintenance workflows

## Troubleshooting

**Plugin not found after installation**:
- Verify the plugin path is correct: `claude plugin list`
- Re-install with `claude plugin install --local ./claude-code-plugin`

**Workflow does not resume previous session**:
- Check that `aidlc-docs/aidlc-state.md` exists in your project root
- Ensure you are running `/aidlc` from the same directory as the original session

**Questions not appearing**:
- For 1-3 questions, the workflow uses the AskUserQuestion tool (interactive prompt)
- For 4+ questions, check for new `.md` files in `aidlc-docs/`

**Build or test commands fail**:
- Verify your development environment has the required tools installed
- Check `aidlc-docs/construction/build-and-test/build-instructions.md` for prerequisites

## More Information

See the [main repository README](../README.md) for full documentation on AI-DLC workflows.

## License

MIT-0
