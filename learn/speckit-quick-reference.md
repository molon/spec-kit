# Spec Kit Quick Reference Guide

## What is Spec Kit?

Spec Kit is a **Spec-Driven Development (SDD)** toolkit that helps development teams define clear specifications before writing code. Through a series of AI-assisted commands, it transforms natural language feature descriptions into structured specifications, implementation plans, and task lists.

### Core Philosophy

- **Spec First, Code Later**: Clarify "what" and "why" before implementation
- **Constitution Constraints**: Project constitution defines core principles that all artifacts must follow
- **Progressive Refinement**: Vague description → Specification → Plan → Tasks → Implementation
- **AI-Assisted**: Each phase has dedicated AI command support

## Workflow Diagram

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│                         Spec Kit Development Workflow                        │
└─────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────────────┐
│ Phase 0: Project Initialization (first time or when updating constitution)   │
│ ┌──────────────────────────┐                                                │
│ │  /speckit.constitution   │ ──→ Creates/updates constitution.md            │
│ └──────────────────────────┘     • Defines core principles and constraints  │
│                                  • Executes consistency propagation         │
│                                  • Generates Sync Impact Report             │
└──────────────────────────────────────────────────────────────────────────────┘
                                       │
                              ┌────────▼─────────┐
                              │ Feature Request  │
                              │ (Natural Lang.)  │
                              └────────┬─────────┘
                                       │
                                       ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│ Phase 1: Specification Definition                                            │
│ ┌────────────────────┐                                                       │
│ │  /speckit.specify  │ ──→ Creates spec.md + checklists/requirements.md     │
│ └────────────────────┘     • User scenarios and tests                        │
│          │                 • Functional requirements, success criteria       │
│          ▼                                                                   │
│ ┌────────────────────┐                                                       │
│ │  /speckit.clarify  │ ──→ Identifies ambiguities, asks questions (optional) │
│ └────────────────────┘                                                       │
└──────────────────────────────────────────────────────────────────────────────┘
                                       │
                                       ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│ Phase 2: Technical Planning                                                  │
│ ┌────────────────────┐                                                       │
│ │   /speckit.plan    │ ──→ Creates plan.md (Implementation Plan)             │
│ └────────────────────┘     • Reads constitution.md (Constitution Check)      │
│                            • Technical context and architecture              │
│                            • Data model, API contracts, research docs        │
└──────────────────────────────────────────────────────────────────────────────┘
                                       │
                                       ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│ Phase 3: Task Breakdown                                                      │
│ ┌────────────────────┐                                                       │
│ │   /speckit.tasks   │ ──→ Creates tasks.md (Task List)                      │
│ └────────────────────┘     • Organized by user story, dependency-ordered     │
│          │                 • Parallel execution markers [P]                  │
│          ▼                                                                   │
│ ┌────────────────────┐                                                       │
│ │ /speckit.checklist │ ──→ Creates domain-specific checklists (on demand)    │
│ └────────────────────┘     • e.g., ux.md, security.md, api.md                │
└──────────────────────────────────────────────────────────────────────────────┘
                                       │
                                       ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│ Phase 4: Quality Verification                                                │
│ ┌────────────────────┐                                                       │
│ │  /speckit.analyze  │ ──→ Consistency Analysis Report                       │
│ └────────────────────┘     • Reads constitution.md (Constitution Validation) │
│                            • Checks spec ↔ plan ↔ tasks consistency          │
│                            • Identifies duplications, ambiguities, gaps      │
│                            • ⚠️ Read-only operation, no file modifications   │
└──────────────────────────────────────────────────────────────────────────────┘
                                       │
                          ┌────────────┴────────────┐
                          ▼                         ▼
┌─────────────────────────────────────┐   ┌─────────────────────────────────┐
│ Phase 5a: Create GitHub Issues      │   │ Phase 5b: Direct Implementation │
│ ┌─────────────────────────────┐     │   │ ┌─────────────────────────────┐ │
│ │  /speckit.taskstoissues     │     │   │ │   /speckit.implement        │ │
│ └─────────────────────────────┘     │   │ └─────────────────────────────┘ │
│ • Converts tasks to GH Issues       │   │ • Checks checklist status       │
│ • Requires GitHub remote            │   │ • Loads implementation context  │
│ • Uses GitHub MCP Server            │   │ • Executes tasks by phase       │
│                                     │   │ • Progress tracking & errors    │
└─────────────────────────────────────┘   └─────────────────────────────────┘
```

## Command Details

### `/speckit.constitution` - Create or Update Project Constitution ⭐

**Purpose**: Create or update project constitution and execute consistency propagation to all dependent templates.

**Create vs Update**:

- If `constitution.md` **does not exist** → Creates new constitution
- If `constitution.md` **already exists** → Updates existing constitution
- **Both cases** execute consistency propagation

**Input**: Constitution modification description (natural language)

**Output**:

- Creates/updates `/memory/constitution.md`
- Updates related template files (plan-template.md, spec-template.md, etc.)
- Generates Sync Impact Report

**Main Steps**:

1. Load existing constitution template (or create new one)
2. Collect/derive values for placeholders
3. Draft updated constitution content
4. **Execute consistency propagation checklist**:
   - Updates `/templates/plan-template.md` to ensure Constitution Check aligns
   - Updates `/templates/spec-template.md` to ensure Requirements section aligns with constitution constraints
   - Updates `/templates/tasks-template.md` to ensure task phase organization reflects new principles
   - Updates `/templates/commands/*.md` to verify no outdated references
   - Updates README.md, docs/quickstart.md, etc.
5. Generate Sync Impact Report (version changes, modified principles, templates requiring updates)
6. Validate and write constitution file

**Key Feature**: This is the **core command for consistency propagation**. Run this command after modifying the constitution.

---

### `/speckit.specify` - Create Feature Specification

**Purpose**: Create a structured specification document from natural language feature description.

**Input**: Feature description (natural language)

**Output**:

- `specs/[###-feature]/spec.md` - Feature specification
- `specs/[###-feature]/checklists/requirements.md` - Spec quality checklist

**Main Steps**:

1. Generate short branch name (2-4 words)
2. Check existing branches, determine next available number
3. Create and checkout new branch
4. Generate specification using `spec-template.md`
5. Validate spec quality
6. If ambiguities exist (max 3), ask user for clarification

**Does NOT involve**: Constitution check (handled by `/speckit.plan`)

---

### `/speckit.clarify` - Clarify Spec Requirements

**Purpose**: Identify ambiguities and underspecified areas in the specification through questions.

**Input**: Existing `spec.md`

**Output**: Updated `spec.md` (ambiguities resolved)

**Main Steps**:

1. Read existing specification
2. Identify underspecified areas
3. Generate clarification questions
4. Record user answers
5. Update specification

**Does NOT involve**: Constitution check

---

### `/speckit.plan` - Create Implementation Plan

**Purpose**: Create technical implementation plan based on specification.

**Input**: Auto-detects feature directory from current branch, reads `spec.md` from it

- **No need** to manually specify feature name

**Output**:

- `specs/[###-feature]/plan.md` - Implementation plan
- `specs/[###-feature]/research.md` - Research documentation
- `specs/[###-feature]/data-model.md` - Data model
- `specs/[###-feature]/contracts/` - API contracts
- `specs/[###-feature]/quickstart.md` - Quick start guide

**Main Steps**:

1. Read `spec.md` and **`/memory/constitution.md`**
2. Fill technical context
3. **Fill Constitution Check section**
4. **Evaluate constitution gates** (ERROR if violations unjustified)
5. Phase 0: Generate research documentation
6. Phase 1: Generate data model, API contracts
7. **Re-evaluate Constitution Check**

**Do you need to manually modify the generated plan?**

- **Usually not**: Generated plan.md should be complete
- **Exception**: If there are "NEEDS CLARIFICATION" markers, you need to fill in the information
- **Best practice**: Run `/speckit.analyze` first to check, modify or regenerate if issues found

**Can generated artifacts be adjusted?**

**Yes, this is expected behavior.** One of Spec Kit's core principles is **Continuous Refinement**:

> "Consistency validation happens continuously, not as a one-time gate. AI analyzes specifications for ambiguity, contradictions, and gaps as an ongoing process."

If you're not satisfied with the content of generated documents like `research.md`, `data-model.md`, `contracts/`, you can:

1. **Communicate directly with AI to adjust**: Describe what needs to be modified, let AI regenerate or modify
2. **Edit manually**: Directly modify file contents
3. **Re-run the command**: Provide more detailed input, regenerate the document

This iterative refinement process is part of Spec Kit's design, ensuring that final specifications and design documents accurately reflect project requirements.

**Key Feature**: This command **reads constitution when generating artifacts** (`/speckit.analyze` also reads constitution for validation).

---

### `/speckit.tasks` - Generate Task List

**Purpose**: Generate executable task list based on design artifacts.

**Input**: `plan.md`, `spec.md`, and other design documents

**Output**: `specs/[###-feature]/tasks.md`

**Main Steps**:

1. Load design documents (plan.md, spec.md, data-model.md, etc.)
2. Organize tasks by user story
3. Generate dependency graph
4. Mark parallelizable tasks [P]
5. Validate task completeness

**Task Format**:

```text
- [ ] T001 [P] [US1] Description file-path
```

**Does NOT involve**: Direct constitution reading

---

### `/speckit.checklist` - Create Checklist

**Purpose**: Create custom checklists for specific domains ("unit tests for requirements").

**Input**: Domain description (optional, e.g., "UX design", "security", "performance")

- If input provided: Directly generates checklist for that domain
- If no input: Will ask 2-3 clarifying questions before generating

**Output**: `specs/[###-feature]/checklists/[domain].md`

**Difference from `/speckit.specify`**:

- `/speckit.specify` **automatically** generates `requirements.md` (spec quality check)
- `/speckit.checklist` generates domain-specific checklists **on demand** (e.g., ux.md, security.md)
- They **do not overlap**, different responsibilities

**Does NOT involve**: Constitution check

---

### `/speckit.analyze` - Consistency Analysis ⭐

**Purpose**: Perform **read-only** consistency and quality analysis on spec.md, plan.md, tasks.md.

**Input**: `spec.md`, `plan.md`, `tasks.md`, `constitution.md`

**Output**: Analysis report (no file writes)

**Analysis Content**:

| Category | Checks |
|----------|--------|
| Duplication Detection | Near-duplicate requirements |
| Ambiguity Detection | Vague adjectives, unresolved placeholders |
| Underspecification | Requirements missing measurable outcomes |
| **Constitution Alignment** | Conflicts with MUST principles, missing required sections |
| Coverage Gaps | Requirements without tasks, tasks without requirements |
| Inconsistency | Terminology drift, data entity reference mismatches |

**Severity Levels**:

- **CRITICAL**: Violates constitution MUST, missing core artifacts
- **HIGH**: Duplicate/conflicting requirements, untestable acceptance criteria
- **MEDIUM**: Terminology drift, missing non-functional task coverage
- **LOW**: Style/wording improvements

**Key Features**:

- ⚠️ **Read-only operation**, does not modify any files
- Constitution conflicts automatically marked as CRITICAL
- Must run after `/speckit.tasks`

---

### `/speckit.taskstoissues` - Convert to GitHub Issues

**Purpose**: Convert tasks from tasks.md into GitHub Issues.

**Prerequisites**:

- Git remote must be a GitHub URL
- Requires GitHub MCP Server

**Input**: `tasks.md`

**Output**: GitHub Issues (created in corresponding repository)

**Main Steps**:

1. Read tasks.md
2. Get Git remote URL
3. Verify it's a GitHub repository
4. Create GitHub Issue for each task

---

### `/speckit.implement` - Execute Implementation

**Purpose**: Execute implementation plan according to tasks.md.

**Input**: `tasks.md`, `plan.md`, and other design documents

**Output**: Implemented code

**Main Steps**:

1. **Check checklist status** (scans checklists/ directory, counts completion)
   - If incomplete items → Shows status table and asks whether to continue
   - All complete → Automatically continues
2. Load implementation context
3. Project setup verification (create .gitignore, etc.)
4. Parse task structure
5. **Execute tasks by phase**
6. Progress tracking and error handling
7. Completion validation

**Execution Rules**:

- Execute phases sequentially
- Respect dependencies
- Tasks marked [P] can run in parallel
- TDD approach: tests before implementation
- Mark completed tasks as [X]

**Key Feature**: This is the **only command that checks checklists/**.

---

## Constitution and Consistency

### Three-Layer Responsibility Model

Spec Kit separates concerns into three layers:

| Layer            | Role                   | Declares                                                                                                   |
| ---------------- | ---------------------- | ---------------------------------------------------------------------------------------------------------- |
| **Constitution** | WHAT constraints exist | One-line MUST/SHOULD principles (consumed by `/speckit.plan` and `/speckit.analyze`)                       |
| **Templates**    | HOW to structure/check | Expand principles into check items and artifact structure (updated by `/speckit.constitution` propagation) |
| **Skills**       | HOW to implement       | Code-level patterns and examples for AI passive perception (not consumed by speckit commands)              |

- Constitution declares the constraint (e.g., "Tests MUST use real PostgreSQL")
- Templates define how to structure artifacts and check compliance (e.g., plan-template's Constitution Check, spec-template's Requirements, tasks-template's phases)
- Skills show concrete code patterns (e.g., `go-testing-patterns` with testcontainers setup)

> **Note**: `/speckit.constitution` propagates changes to plan-template, spec-template, and tasks-template. Checklist-template is NOT part of this propagation — it validates requirement quality independently.

### Which Commands Read/Update Constitution?

| Command                  | Constitution | Purpose                                                |
| ------------------------ | ------------ | ------------------------------------------------------ |
| `/speckit.constitution`  | ✅ Updates   | Create/update constitution + propagate to templates ⭐ |
| `/speckit.specify`       | ❌           | -                                                      |
| `/speckit.clarify`       | ❌           | -                                                      |
| `/speckit.plan`          | ✅ Reads     | Fill constitution check, evaluate gates                |
| `/speckit.tasks`         | ❌           | -                                                      |
| `/speckit.checklist`     | ❌           | -                                                      |
| `/speckit.analyze`       | ✅ Reads     | Validate constitution alignment                        |
| `/speckit.taskstoissues` | ❌           | -                                                      |
| `/speckit.implement`     | ❌           | -                                                      |

### Which Commands Check Checklists?

| Command                  | Checks Checklists | Notes                                                     |
| ------------------------ | ----------------- | --------------------------------------------------------- |
| `/speckit.analyze`       | ❌                | Only analyzes spec/plan/tasks consistency, not checklists |
| `/speckit.implement`     | ✅                | Checks checklists/ before execution, asks if incomplete   |
| `/speckit.taskstoissues` | ❌                | Only reads tasks.md to create Issues, not checklists      |

> **Note**: There is currently no command dedicated to only checking checklists. To check separately, manually review the `checklists/` directory.

### Checklist Creation and Updates

| Checklist Type                  | Created By           | Auto-updates Status                         | Manual Marking                   |
| ------------------------------- | -------------------- | ------------------------------------------- | -------------------------------- |
| `requirements.md`               | `/speckit.specify`   | ✅ Auto-updates pass/fail during validation | Not needed                       |
| Domain checklists (ux.md, etc.) | `/speckit.checklist` | ❌ No                                       | ✅ Requires manual `[X]` marking |

**Notes**:

- `/speckit.specify` auto-generates `requirements.md` and automatically updates each item's pass/fail status during validation
- `/speckit.checklist` creates domain checklists (e.g., ux.md, security.md) that require users to manually mark completion (`[ ]` → `[X]`) after review
- `/speckit.implement` only reads checklist status for statistics, does not modify checklist files

### Actions After Constitution Update

**Recommended Flow**: Use `/speckit.constitution` command to modify constitution

```bash
# 1. Run constitution command, describe your changes
/speckit.constitution Add new security principle: all user input must be validated

# The command will automatically:
# - Update /memory/constitution.md
# - Execute consistency propagation checklist
# - Update related template files
# - Generate sync impact report
# - Update version number
```

**If you directly edited the constitution file**:

```bash
# 1. Run constitution command to trigger consistency propagation
/speckit.constitution Confirm and propagate recent constitution changes

# 2. Check if existing artifacts comply with new constitution
/speckit.analyze
```

**Follow-up Steps**:

1. Run `/speckit.analyze` to check if existing artifacts comply with new constitution
2. If violations found, run `/speckit.plan` to regenerate plan
3. Run `/speckit.tasks` to regenerate tasks

**New features automatically use new constitution**: Subsequently created features will automatically use the new constitution, `/speckit.plan` will read the new constitution and apply gates.

### Constitution Check in Plan Template

The plan template (`plan-template.md`) contains a "Constitution Check" section:

```markdown
## Constitution Check

_Gating: Must pass before Phase 0 research. Re-check after Phase 1 design._

[Gates determined based on constitution file]
```

- `/speckit.plan` command fills this section based on the constitution file
- Gates must pass before Phase 0 research
- Re-check required after Phase 1 design

### Constitution Dependency Diagram

```text
constitution.md (Source of Truth)
    │
    ├─→ /speckit.constitution command (read + update + consistency propagation) ⭐
    │   ├─→ Updates constitution.md
    │   ├─→ Updates templates/plan-template.md
    │   ├─→ Updates templates/spec-template.md
    │   ├─→ Updates templates/tasks-template.md
    │   ├─→ Updates templates/commands/*.md
    │   └─→ Generates sync impact report
    │
    ├─→ /speckit.plan command (reads directly)
    │   └─→ specs/*/plan.md (contains Constitution Check section)
    │
    └─→ /speckit.analyze command (reads directly)
        └─→ Consistency report (includes constitution alignment issues)

Note: The following commands do NOT directly read constitution:
- /speckit.specify
- /speckit.clarify
- /speckit.tasks
- /speckit.checklist
- /speckit.implement
- /speckit.taskstoissues
```

## Directory Structure

```text
project/
├── .specify/                        # Spec Kit core directory (created by specify init)
│   ├── memory/
│   │   └── constitution.md          # Project constitution (core principles & constraints)
│   ├── scripts/                     # Helper scripts
│   │   └── bash/
│   │       ├── check-prerequisites.sh
│   │       ├── common.sh
│   │       ├── create-new-feature.sh
│   │       ├── setup-plan.sh
│   │       └── update-agent-context.sh
│   └── templates/                   # Template files
│       ├── agent-file-template.md   # AI Agent rules template (used by update-agent-context.sh)
│       ├── checklist-template.md    # Checklist template
│       ├── plan-template.md         # Implementation plan template
│       ├── spec-template.md         # Feature spec template
│       └── tasks-template.md        # Task list template
├── CLAUDE.md                        # Claude Code context rules (generated/updated by update-agent-context.sh)
├── .claude/                         # Claude Code integration (other AI tools have corresponding dirs)
│   └── skills/                      # Claude Skills (command definitions)
│       ├── speckit-analyze/
│       │   └── SKILL.md             # Consistency analysis command
│       ├── speckit-checklist/
│       │   └── SKILL.md             # Checklist command
│       ├── speckit-clarify/
│       │   └── SKILL.md             # Clarify spec command
│       ├── speckit-constitution/
│       │   └── SKILL.md             # Constitution management command
│       ├── speckit-implement/
│       │   └── SKILL.md             # Implementation execution command
│       ├── speckit-plan/
│       │   └── SKILL.md             # Implementation plan command
│       ├── speckit-specify/
│       │   └── SKILL.md             # Spec definition command
│       ├── speckit-tasks/
│       │   └── SKILL.md             # Task generation command
│       └── speckit-taskstoissues/
│           └── SKILL.md             # Tasks to Issues command
└── specs/                           # Feature specs directory (generated at runtime)
    └── 001-feature-name/            # Feature directory (created by /speckit.specify)
        ├── spec.md                  # Feature specification (/speckit.specify)
        ├── plan.md                  # Implementation plan (/speckit.plan)
        ├── tasks.md                 # Task list (/speckit.tasks)
        ├── research.md              # Research docs (/speckit.plan)
        ├── data-model.md            # Data model (/speckit.plan)
        ├── quickstart.md            # Quick start (/speckit.plan)
        ├── contracts/               # API contracts (/speckit.plan)
        │   └── api.md
        └── checklists/              # Checklists
            ├── requirements.md      # Spec quality check (/speckit.specify auto)
            ├── ux.md                # UX check (/speckit.checklist on demand)
            └── security.md          # Security check (/speckit.checklist on demand)
```

## Design Artifacts Explained

The `/speckit.plan` command generates multiple design artifacts in Phase 1, which are used by different commands in subsequent workflows.

### research.md - Research Documentation

**Generated by**: `/speckit.plan` Phase 0

**Contents**:

- Technical decisions and their rationale
- Alternatives considered
- Resolution of all "NEEDS CLARIFICATION" markers

**Used by**:

- `/speckit.plan` Phase 1 (as prerequisite)
- `/speckit.tasks` (extract decisions for setup tasks)
- `/speckit.implement` (get technical decisions and constraints)

---

### data-model.md - Data Model

**Generated by**: `/speckit.plan` Phase 1

**Contents**:

- Entity names, fields, relationships
- Validation rules from requirements
- State transitions (if applicable)

**Used by**:

- `/speckit.tasks` (extract entities and map to user stories)
- `/speckit.implement` (get entities and relationships)

---

### contracts/ - API Contracts

**Generated by**: `/speckit.plan` Phase 1

**Contents**:

- API endpoints generated from functional requirements
- OpenAPI/GraphQL schemas
- One endpoint per user action

**Used by**:

- `/speckit.tasks` (map endpoints to user stories)
- `/speckit.implement` (get API specifications and test requirements)
- `/speckit.checklist` (extract API-related signals)

---

### quickstart.md - Quick Start

**Generated by**: `/speckit.plan` Phase 1

**Contents**:

- Integration scenarios
- Quick start guide
- Test scenarios

**Used by**:

- `/speckit.tasks` (get test scenarios)
- `/speckit.implement` (get integration scenarios)

---

### Artifact Flow Diagram

```text
                        ┌─────────────────────────────────────┐
                        │        /speckit.constitution        │
                        │  Read/Update: constitution.md       │
                        │  Propagate to: templates, cmds, docs│
                        └──────────────┬──────────────────────┘
                                       │
                                       ▼
┌──────────────────────────────────────────────────────────────────────────┐
│                              Main Flow                                    │
├──────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  User Requirements                                                       │
│      │                                                                   │
│      ▼                                                                   │
│  /speckit.specify ─────────────────────────────────────────────────────┐ │
│      │                                                                 │ │
│      ├─→ spec.md                                                       │ │
│      └─→ checklists/requirements.md                                    │ │
│      │                                                                 │ │
│      ▼                                                                 │ │
│  /speckit.clarify (optional)                                           │ │
│      │                                                                 │ │
│      └─→ Update spec.md                                                │ │
│      │                                                                 │ │
│      ▼                                                                 │ │
│  /speckit.plan ◀── Read: spec.md, constitution.md                      │ │
│      │                                                                 │ │
│      ├─→ research.md (Phase 0)                                         │ │
│      ├─→ data-model.md, contracts/, quickstart.md (Phase 1)            │ │
│      └─→ Auto-trigger update-agent-context.sh → CLAUDE.md              │ │
│      │                                                                 │ │
│      ▼                                                                 │ │
│  /speckit.tasks ◀── Read: plan.md, spec.md, [data-model, contracts...] │ │
│      │                                                                 │ │
│      └─→ tasks.md                                                      │ │
│      │                                                                 │ │
│      ▼                                                                 │ │
│  /speckit.analyze (recommended) ◀── Read: spec, plan, tasks, const.    │ │
│      │                                                                 │ │
│      └─→ Consistency validation report                                 │ │
│      │                                                                 │ │
│      ├────────────────────────┬────────────────────────────────────────┘ │
│      │                        │                                          │
│      ▼                        ▼                                          │
│  /speckit.taskstoissues   /speckit.implement                             │
│  (team collaboration)     (solo development)                             │
│      │                        │                                          │
│      └─→ GitHub Issues        ├─→ Check checklists/*                     │
│                               ├─→ Read tasks, plan, [data-model...]      │
│                               └─→ Code implementation                    │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────────┐
│  /speckit.checklist (optional, can be used anytime after specify)        │
│      │                                                                   │
│      ├─→ Read: spec.md, plan.md, tasks.md (if exists)                    │
│      └─→ Output: checklists/ux.md, checklists/security.md, etc.          │
│                                                                          │
│  💡 Recommended to complete checklist marking before implement,          │
│     as implement checks checklist status                                 │
└──────────────────────────────────────────────────────────────────────────┘
```

**Official Recommended Execution Order** (from quickstart.md):

```text
constitution → specify → clarify → plan → tasks → analyze → implement
                                                     ↑
                                            checklist (optional, anytime)
```

## AI Agent Context Update

### What is Agent Context?

Spec Kit supports multiple AI coding assistants (Claude, Windsurf, Cursor, Copilot, etc.). Each AI tool has its own rules file that provides project context information to the AI (tech stack, directory structure, recent changes, etc.).

### Related Files

| File                      | Location                 | Description                       |
| ------------------------- | ------------------------ | --------------------------------- |
| `agent-file-template.md`  | `.specify/templates/`    | Template for AI Agent rules files |
| `CLAUDE.md`               | Project root             | Generated AI Agent context file (Claude Code) |
| `update-agent-context.sh` | `.specify/scripts/bash/` | Update script                     |

### Workflow

```text
plan.md (project metadata source)
    │
    ▼
update-agent-context.sh (parses plan.md)
    │
    ├─→ Reads agent-file-template.md (template)
    │
    └─→ Generates/updates context files for each AI tool
        ├─→ CLAUDE.md (Claude Code)
        ├─→ .cursor/rules/specify-rules.mdc (Cursor)
        ├─→ .windsurf/rules/specify-rules.md (Windsurf)
        └─→ Other AI tool files...
```

### Trigger Timing

The `update-agent-context.sh` script is **automatically triggered** by **`/speckit.plan`** after Phase 1 completion:

```yaml
# Defined in plan.md command template
agent_scripts:
  sh: scripts/bash/update-agent-context.sh __AGENT__
  ps: scripts/powershell/update-agent-context.ps1 -AgentType __AGENT__
```

Can also be run manually:

```bash
# Update all existing AI Agent rules files
./.specify/scripts/bash/update-agent-context.sh

# Update only a specific AI tool's rules file
./.specify/scripts/bash/update-agent-context.sh claude
./.specify/scripts/bash/update-agent-context.sh windsurf
```

### Manual Run Scenarios

- After project tech stack changes outside of `plan.md`
- When you need to update a specific AI tool's rules file separately
- When debugging or testing AI Agent context

## Best Practices

### 1. Complete Workflow

```bash
# 1. Create specification
/speckit.specify I want to add user authentication feature

# 2. Clarify ambiguities (optional)
/speckit.clarify

# 3. Create implementation plan
/speckit.plan

# 4. Generate task list
/speckit.tasks

# 5. Consistency analysis (recommended)
/speckit.analyze

# 6a. Create GitHub Issues (team collaboration)
/speckit.taskstoissues

# 6b. Or implement directly (individual development)
/speckit.implement
```

### 2. Run Analysis Before Implementation

Always run `/speckit.analyze` before `/speckit.implement` to ensure:

- All requirements have corresponding tasks
- No constitution violations
- No significant ambiguities or underspecification

### 3. Constitution is Immutable

Do not modify constitution during implementation. If changes needed:

1. Complete current feature
2. Update constitution
3. Run `/speckit.analyze` to check impact
4. Update affected artifacts

### 4. Verify Consistency in PRs (Manual Best Practice)

> **Note**: This is a **manual best practice recommendation**. Spec Kit currently has no CI integration or automated PR checks.

Before submitting a PR, it's recommended to:

1. Manually run `/speckit.analyze` to check consistency
2. Manually add the following checklist to PR description:

```markdown
## PR Checklist (Manual)

- [ ] Ran /speckit.analyze to check constitution alignment
- [ ] Plan passes all constitution gates
- [ ] No CRITICAL level constitution violations
```

### 5. Constitution Version Control

`/speckit.constitution` command automatically manages constitution version numbers following semantic versioning:

- **MAJOR**: Backward incompatible governance/principle removals or redefinitions
- **MINOR**: New principle/section added or materially expanded guidance
- **PATCH**: Clarifications, wording, typo fixes, non-semantic refinements

## Limitations and Notes

### Current Limitations

1. **No automated propagation**: Constitution updates don't automatically update existing artifacts (spec/plan/tasks)
2. **No automatic notifications**: Constitution updates don't automatically notify developers
3. **No git hooks**: No git hooks to automatically trigger consistency checks
4. **Some commands don't read constitution**: `/speckit.specify`, `/speckit.clarify`, etc. don't directly read constitution

### Important Notes

1. **Manual verification is required**: After constitution updates, manually run `/speckit.analyze`
2. **Existing artifacts need manual updates**: After constitution updates, manually regenerate affected artifacts
3. **Constitution conflicts are CRITICAL**: `/speckit.analyze` marks constitution conflicts as CRITICAL level

---

## Extension System

Extensions are Spec Kit's **user-facing modular add-on mechanism** for integrating external tools (Jira, Linear, etc.) or adding custom workflows, without modifying the core spec-kit.

### Core Concepts

- Extensions are installed to `.specify/extensions/<ext-id>/`
- Each extension has a declarative `extension.yml` manifest
- Extensions can **add commands** (registered into AI Agent command directories)
- Extensions can **register hooks** (triggered automatically after core commands)
- Extensions can **provide templates** (participate in the four-layer resolution stack)

### extension.yml Manifest Structure

```yaml
schema_version: "1.0"

extension:
  id: "jira"                      # Unique identifier (lowercase, hyphens)
  name: "Jira Integration"
  version: "1.0.0"
  description: "Create Jira Epics/Stories/Issues from spec-kit artifacts"
  author: "Your Org"
  repository: "https://github.com/your-org/spec-kit-jira"
  license: "MIT"

requires:
  speckit_version: ">=0.1.0,<2.0.0"   # Compatible spec-kit version range
  tools:                               # External tool dependencies (optional)
    - name: "jira-mcp-server"
      required: true

provides:
  commands:                            # New AI commands provided
    - name: "speckit.jira.specstoissues"   # Naming: speckit.<ext>.<cmd>
      file: "commands/specstoissues.md"
      description: "Create Jira hierarchy from spec and tasks"

hooks:                                 # Hooks registered on core commands (optional)
  after_tasks:
    command: "speckit.jira.specstoissues"
    optional: true
    prompt: "Create Jira issues from tasks?"

tags:
  - "issue-tracking"
  - "jira"
```

### Command Naming Convention

Extension commands follow the `speckit.<ext-id>.<command>` format:

```text
/speckit.jira.specstoissues     ← command from jira extension
/speckit.linear.sync            ← command from linear extension
/speckit.checkpoint.save        ← command from checkpoint extension
```

After installation, the CLI automatically registers commands into all installed AI Agent directories (`.claude/commands/`, `.gemini/commands/`, etc.).

### CLI Command Reference

```bash
# Discovery
specify extension search                       # List all available extensions
specify extension search jira                  # Search by keyword
specify extension search --tag issue-tracking  # Search by tag
specify extension info jira                    # Show details

# Installation
specify extension add jira                     # Install from official/community catalog
specify extension add --from <zip-url>         # Install from URL (bypasses catalog restriction)
specify extension add --dev /path/to/ext       # Install from local directory (dev mode)

# Management
specify extension list                         # List installed extensions
specify extension remove jira                  # Uninstall
specify extension update jira                  # Update to latest version
specify extension update --all                 # Update all
specify extension enable jira                  # Enable
specify extension disable jira                 # Disable (preserves files)
specify extension set-priority jira 5          # Set priority (affects template resolution order)
```

### Extension Installation Flow

```text
specify extension add jira
    ↓
1. Resolve download URL from catalog
2. Download ZIP package
3. Validate manifest & check compatibility
4. Extract to .specify/extensions/jira/
5. Register commands into all AI Agent directories
6. Record metadata in .specify/extensions/.registry
7. Register hooks in .specify/extensions.yml (if any)
```

---

## Hook System

Hooks are extension points that **trigger automatically after core commands complete**, defined by extensions in `extension.yml` and written to `.specify/extensions.yml` at install time.

### Supported Hook Points

All core commands support both `before_*` and `after_*` hooks:

| Hook Point | Trigger Timing |
|-----------|---------------|
| `before_specify` / `after_specify` | Before/after `/speckit.specify` |
| `before_plan` / `after_plan` | Before/after `/speckit.plan` |
| `before_tasks` / `after_tasks` | Before/after `/speckit.tasks` |
| `before_implement` / `after_implement` | Before/after `/speckit.implement` |
| `before_analyze` / `after_analyze` | Before/after `/speckit.analyze` |
| `before_checklist` / `after_checklist` | Before/after `/speckit.checklist` |

### .specify/extensions.yml Structure

Hook registrations written here during extension installation:

```yaml
hooks:
  after_tasks:
    - extension: jira
      command: speckit.jira.specstoissues
      enabled: true
      optional: true
      prompt: "Create Jira issues from tasks?"

  after_implement:
    - extension: jira
      command: speckit.jira.sync-status
      enabled: true
      optional: true
      prompt: "Sync completion status to Jira?"
```

### Hook Execution Mechanism

Hooks are check logic **embedded at the end of core command templates** (executed at the AI level, not CLI level):

```text
/speckit.tasks completes
    ↓
Core command tail: check after_tasks hooks in .specify/extensions.yml
    ↓
Found jira's after_tasks hook (optional: true)
    ↓
AI prompts user: "Create Jira issues from tasks?"
    ├── User answers y → AI executes /speckit.jira.specstoissues
    └── User answers n → skip
```

**Note**: Hooks with `optional: true` prompt the user; hooks without it (or `optional: false`) auto-execute. Disabled extensions also have their hooks skipped.

### Extension Configuration Layers

Extension configuration is merged in priority order (highest to lowest):

```text
Environment variables (SPECKIT_<EXT>_*)                           ← highest
    ↓
.specify/extensions/<ext>/<ext>-config.local.yml  ← local overrides (gitignored)
    ↓
.specify/extensions/<ext>/<ext>-config.yml        ← project-level config
    ↓
defaults in extension.yml                         ← extension defaults
```

---

## Template & Command Override System

### Four-Layer Priority Resolution

Spec Kit resolves templates and commands using the following priority order (highest to lowest; first match wins):

```text
Priority (highest to lowest):

1. .specify/templates/overrides/           ← Project-local overrides (highest priority)
2. .specify/presets/<preset-id>/           ← Installed presets
3. .specify/extensions/<ext-id>/templates/ ← Extension-provided templates
4. .specify/templates/                     ← Core templates (Spec Kit defaults)
```

### Override Scope

**Both templates and commands support overrides**, all placed under `.specify/templates/overrides/`:

| Type | Override Path | Example |
|------|--------------|---------|
| Template files | `.specify/templates/overrides/<name>.md` | `overrides/spec-template.md` |
| Command files | `.specify/templates/overrides/<name>.md` | `overrides/speckit.specify.md` |
| Script files | `.specify/templates/overrides/scripts/<name>.sh` | `overrides/scripts/create-new-feature.sh` |

### Partial Overrides

**You can override only some templates** — no need to override all of them. The resolver uses first-match logic: files present in the overrides directory use the override version; files not present fall through to the next layer. For example, creating just two files overrides only those two templates:

```text
.specify/templates/overrides/
  spec-template.md      ← only these two are overridden
  plan-template.md      ← remaining 4 still use default versions
```

### Verify Override

```bash
# Check which file a template actually resolves to (name without file extension)
specify preset resolve spec-template
specify preset resolve speckit.specify
```

> **Note**: `resolve` accepts names **without file extensions** (do not append `.md`). Adding a suffix will result in no match.

### Relationship with Presets

- **overrides/**: One-off customization for a single project; highest priority
- **Preset**: Packaged set of overrides for cross-project reuse; installed via `specify preset add`
- Both can coexist; overrides always take precedence over presets
