# Spec Kit Quick Reference Guide

## What is Spec Kit?

Spec Kit is a **Spec-Driven Development (SDD)** toolkit that helps development teams define clear specifications before writing code. Through a series of AI-assisted commands, it transforms natural language feature descriptions into structured specifications, implementation plans, and task lists.

### Core Philosophy

- **Spec First, Code Later**: Clarify "what" and "why" before implementation
- **Constitution Constraints**: Project constitution defines core principles that all artifacts must follow
- **Progressive Refinement**: Vague description → Specification → Plan → Tasks → Implementation
- **AI-Assisted**: Each phase has dedicated AI command support

## Workflow Diagram

```
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

```
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

```
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

```
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
│       ├── agent-file-template.md   # AI Agent rules template
│       ├── checklist-template.md    # Checklist template
│       ├── plan-template.md         # Implementation plan template
│       ├── spec-template.md         # Feature spec template
│       └── tasks-template.md        # Task list template
├── .windsurf/                       # Windsurf IDE integration (other AI tools have corresponding dirs)
│   ├── rules/                       # Windsurf rules
│   │   └── specify-rules.md         # Spec Kit rules definition
│   └── workflows/                   # Windsurf workflows (command definitions)
│       ├── speckit.analyze.md       # Consistency analysis command
│       ├── speckit.checklist.md     # Checklist command
│       ├── speckit.clarify.md       # Clarify spec command
│       ├── speckit.constitution.md  # Constitution management command
│       ├── speckit.implement.md     # Implementation execution command
│       ├── speckit.plan.md          # Implementation plan command
│       ├── speckit.specify.md       # Spec definition command
│       ├── speckit.tasks.md         # Task generation command
│       └── speckit.taskstoissues.md # Tasks to Issues command
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
