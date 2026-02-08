# Workspace Detection

**Purpose**: Determine workspace state and check for existing AI-DLC projects

## Step 1: Check for Existing AI-DLC Project

Check if `aidlc-docs/aidlc-state.md` exists:
- **If exists**: Resume from last phase (load context from previous phases)
- **If not exists**: Continue with new project assessment

## Step 2: Scan Workspace for Existing Code

**Determine if workspace has existing code:**
- Scan workspace for source code files (.java, .py, .js, .ts, .jsx, .tsx, .kt, .kts, .scala, .groovy, .go, .rs, .rb, .php, .c, .h, .cpp, .hpp, .cc, .cs, .fs, etc.)
- Check for build files (pom.xml, package.json, build.gradle, etc.)
- Look for project structure indicators
- Identify workspace root directory (NOT aidlc-docs/)

**Record findings:**
```markdown
## Workspace State
- **Existing Code**: [Yes/No]
- **Programming Languages**: [List if found]
- **Build System**: [Maven/Gradle/npm/etc. if found]
- **Project Structure**: [Monolith/Microservices/Library/Empty]
- **Workspace Root**: [Absolute path]
```

### Claude Code Tool Note: Workspace Scanning Patterns

Use Glob and Grep tools for efficient workspace detection:

**Source file detection** (use Glob):
- `**/*.{ts,tsx,js,jsx}` -- TypeScript/JavaScript projects
- `**/*.{java,kt,scala}` -- JVM-based projects
- `**/*.{py,pyi}` -- Python projects
- `**/*.{go}` -- Go projects
- `**/*.{rs}` -- Rust projects
- `**/*.{cs,fs}` -- .NET projects
- `**/*.{rb}` -- Ruby projects
- `**/*.{cpp,cc,c,h,hpp}` -- C/C++ projects

**Build system detection** (use Glob):
- `**/package.json` -- Node.js/npm
- `**/pom.xml` -- Maven
- `**/build.gradle`, `**/build.gradle.kts` -- Gradle
- `**/Cargo.toml` -- Rust/Cargo
- `**/go.mod` -- Go modules
- `**/requirements.txt`, `**/pyproject.toml`, `**/setup.py` -- Python
- `**/Gemfile` -- Ruby/Bundler
- `**/*.csproj`, `**/*.sln` -- .NET

**Framework detection** (use Grep after Glob):
- Search `package.json` for framework dependencies (react, angular, vue, express, nestjs)
- Search `pom.xml` or `build.gradle` for Spring Boot, Quarkus markers
- Search `go.mod` for framework imports (gin, echo, fiber)

Combine multiple Glob calls in parallel to speed up workspace analysis.

## Step 3: Determine Next Phase

**IF workspace is empty (no existing code)**:
- Set flag: `brownfield = false`
- Next phase: Requirements Analysis

**IF workspace has existing code**:
- Set flag: `brownfield = true`
- Check for existing reverse engineering artifacts in `aidlc-docs/inception/reverse-engineering/`
- **IF reverse engineering artifacts exist**: Load them, skip to Requirements Analysis
- **IF no reverse engineering artifacts**: Next phase is Reverse Engineering

## Step 4: Create Initial State File

Create `aidlc-docs/aidlc-state.md`:

```markdown
# AI-DLC State Tracking

## Project Information
- **Project Type**: [Greenfield/Brownfield]
- **Start Date**: [ISO timestamp]
- **Current Stage**: INCEPTION - Workspace Detection

## Workspace State
- **Existing Code**: [Yes/No]
- **Reverse Engineering Needed**: [Yes/No]
- **Workspace Root**: [Absolute path]

## Code Location Rules
- **Application Code**: Workspace root (NEVER in aidlc-docs/)
- **Documentation**: aidlc-docs/ only
- **Structure patterns**: See code-generation.md Critical Rules

## Stage Progress
[Will be populated as workflow progresses]
```

## Step 5: Present Completion Message

**For Brownfield Projects:**
```markdown
# Workspace Detection Complete

Workspace analysis findings:
* **Project Type**: Brownfield project
* [AI-generated summary of workspace findings in bullet points]
* **Next Step**: Proceeding to **Reverse Engineering** to analyze existing codebase...
```

**For Greenfield Projects:**
```markdown
# Workspace Detection Complete

Workspace analysis findings:
* **Project Type**: Greenfield project
* **Next Step**: Proceeding to **Requirements Analysis**...
```

## Step 6: Automatically Proceed

- **No user approval required** - this is informational only
- Automatically proceed to next phase:
  - **Brownfield**: Reverse Engineering (if no existing artifacts) or Requirements Analysis (if artifacts exist)
  - **Greenfield**: Requirements Analysis
