# Consistency Propagation Mechanism

## Overview

Consistency propagation is a core mechanism in Spec Kit that ensures constitution (`/memory/constitution.md`) changes propagate to all dependent templates and artifacts.

**Key Command**: `/speckit.constitution` is the **primary command** for executing consistency propagation. When you modify the constitution, you should run this command to trigger consistency propagation.

Constitution consistency is achieved through:

1. **`/speckit.constitution`**: Executes consistency propagation checklist when updating constitution, updates related templates
2. **`/speckit.plan`**: Reads constitution and applies gates when generating plans
3. **`/speckit.analyze`**: Validates artifact consistency with constitution during analysis

## Core Concepts

### What is Consistency Propagation?

In Spec Kit, consistency propagation refers to **how constitution principles are transmitted to generated artifacts through command execution**. This is not an automated system, but a design principle:

- **Constitution is the source of truth**: Defines core principles and constraints for the project
- **Commands read constitution**: Certain commands read constitution content when executed
- **Artifacts reflect constitution**: Generated artifacts should comply with constitution principles

### Which Commands Read/Update Constitution?

Based on actual command files, the following commands are constitution-related:

1. **`/speckit.constitution`** (`templates/commands/constitution.md`) ⭐ **Core command for consistency propagation**:
   - Creates or updates constitution file
   - **Executes consistency propagation checklist**:
     - Reads `/templates/plan-template.md` to ensure "Constitution Check" aligns with updated principles
     - Reads `/templates/spec-template.md` for scope/requirements alignment
     - Reads `/templates/tasks-template.md` to ensure task categorization reflects new principles
     - Reads `/templates/commands/*.md` to verify no outdated references
     - Reads README.md, docs/quickstart.md, etc. to update principle references
   - **Generates Sync Impact Report**: version changes, modified principles, templates requiring updates
   - Updates version number (follows semantic versioning)

2. **`/speckit.plan`** (`templates/commands/plan.md`):
   - Step 2 explicitly states: `Load context: Read FEATURE_SPEC and /memory/constitution.md`
   - Fills "Constitution Check" section
   - Evaluates constitution gates

3. **`/speckit.analyze`** (`templates/commands/analyze.md`):
   - Step 2 explicitly states: `Load /memory/constitution.md for principle validation`
   - Checks constitution alignment issues
   - Constitution conflicts are marked as CRITICAL

### Which Commands Do NOT Read Constitution?

The following commands **do not directly read** constitution:

- **`/speckit.specify`**: Focuses on creating specifications from user descriptions, does not involve constitution
- **`/speckit.clarify`**: Focuses on identifying ambiguities in specifications, does not involve constitution
- **`/speckit.tasks`**: Generates tasks from plan, does not directly read constitution
- **`/speckit.checklist`**: Generates checklists, does not involve constitution
- **`/speckit.implement`**: Executes tasks, does not directly read constitution
- **`/speckit.taskstoissues`**: Converts tasks to GitHub Issues, does not involve constitution

## `/speckit.constitution` Command Details

This is the **core command** for executing consistency propagation. From actual content of `templates/commands/constitution.md`:

### Main Function

```markdown
You are updating the project constitution at `/memory/constitution.md`. This file is a TEMPLATE
containing placeholder tokens in square brackets (e.g. `[PROJECT_NAME]`, `[PRINCIPLE_1_NAME]`).
Your job is to (a) collect/derive concrete values, (b) fill the template precisely, and
(c) propagate any amendments across dependent artifacts.
```

### Consistency Propagation Checklist (Step 4)

```markdown
4. Consistency propagation checklist (convert prior checklist into active validations):
   - Read `/templates/plan-template.md` and ensure any "Constitution Check" or rules align
     with updated principles.
   - Read `/templates/spec-template.md` for scope/requirements alignment—update if constitution
     adds/removes mandatory sections or constraints.
   - Read `/templates/tasks-template.md` and ensure task categorization reflects new or removed
     principle-driven task types (e.g., observability, versioning, testing discipline).
   - Read each command file in `/templates/commands/*.md` (including this one) to verify no
     outdated references remain when generic guidance is required.
   - Read any runtime guidance docs (e.g., `README.md`, `docs/quickstart.md`, or agent-specific
     guidance files if present). Update references to principles changed.
```

### Sync Impact Report (Step 5)

```markdown
5. Produce a Sync Impact Report (prepend as an HTML comment at top of the constitution file):
   - Version change: old → new
   - List of modified principles (old title → new title if renamed)
   - Added sections
   - Removed sections
   - Templates requiring updates (✅ updated / ⚠ pending) with file paths
   - Follow-up TODOs if any placeholders intentionally deferred.
```

### Version Control

```markdown
`CONSTITUTION_VERSION` must increment according to semantic versioning rules:

- MAJOR: Backward incompatible governance/principle removals or redefinitions.
- MINOR: New principle/section added or materially expanded guidance.
- PATCH: Clarifications, wording, typo fixes, non-semantic refinements.
```

---

## Other Constitution-Related Commands

### `/speckit.plan` Command

From actual content of `templates/commands/plan.md`:

```markdown
2. **Load context**: Read FEATURE_SPEC and `/memory/constitution.md`. Load IMPL_PLAN template (already copied).

3. **Execute plan workflow**: Follow the structure in IMPL_PLAN template to:
   - Fill Technical Context (mark unknowns as "NEEDS CLARIFICATION")
   - Fill Constitution Check section from constitution
   - Evaluate gates (ERROR if violations unjustified)
     ...
   - Re-evaluate Constitution Check post-design
```

**Actual Behavior**:

- Reads constitution file
- Fills "Constitution Check" section in plan template
- Evaluates constitution gates, errors if unjustified violations
- Re-evaluates constitution check after design completion

### `/speckit.analyze` Command

From actual content of `templates/commands/analyze.md`:

```markdown
**From constitution:**

- Load `/memory/constitution.md` for principle validation

...

#### D. Constitution Alignment

- Any requirement or plan element conflicting with a MUST principle
- Missing mandated sections or quality gates from constitution

...

**Constitution Authority**: The project constitution (`/memory/constitution.md`) is **non-negotiable** within this analysis scope. Constitution conflicts are automatically CRITICAL...
```

**Actual Behavior**:

- Reads constitution file for principle validation

## Workflow After Constitution Update

### Recommended Flow: Use `/speckit.constitution`

When you need to modify the constitution, **it's recommended to use the `/speckit.constitution` command** instead of directly editing the file:

```bash
# 1. Run constitution command, describe the changes you want to make
/speckit.constitution Add new security principle: all user input must be validated

# This command will:
# - Update /memory/constitution.md
# - Execute consistency propagation checklist
# - Update related template files
# - Generate Sync Impact Report
# - Update version number
```

### If You Directly Edited the Constitution File

If you directly edited `/memory/constitution.md`, you should run:

```bash
# 1. Run constitution command to trigger consistency propagation
/speckit.constitution Confirm and propagate recent constitution changes

# 2. Check if existing artifacts comply with new constitution
/speckit.analyze
```

### Verify Existing Artifacts

Run `/speckit.analyze` command to check if existing artifacts comply with new constitution:

```bash
/speckit.analyze
```

This will:

- Read the latest constitution
- Check if spec.md, plan.md, tasks.md comply with constitution
- Report any constitution violations (marked as CRITICAL)

### Update Affected Artifacts

If violations are found, regenerate affected artifacts:

```bash
# Regenerate plan (will read new constitution)
/speckit.plan

# Regenerate tasks
/speckit.tasks
```

### New Features Automatically Apply New Constitution

Subsequently created new features will automatically use new constitution:

- `/speckit.plan` will read new constitution and apply gates
- `/speckit.analyze` will use new constitution for validation

## Constitution Check in Plan Template

From actual content of `templates/plan-template.md`:

```markdown
## Constitution Check

_Gating: Must pass before Phase 0 research. Re-check after Phase 1 design._

[Gates determined based on constitution file]
```

**Actual Behavior**:

- Plan template contains a "Constitution Check" section
- `/speckit.plan` command fills this section based on constitution file
- Gates must pass before Phase 0 research
- Re-check required after Phase 1 design

## Dependency Diagram

The following shows **actual** dependencies between constitution and commands:

```
constitution.md (Source of Truth)
    │
    ├─→ /speckit.constitution command (Read + Update + Consistency Propagation) ⭐
    │   ├─→ Updates constitution.md
    │   ├─→ Updates templates/plan-template.md
    │   ├─→ Updates templates/spec-template.md
    │   ├─→ Updates templates/tasks-template.md
    │   ├─→ Updates templates/commands/*.md
    │   └─→ Generates Sync Impact Report
    │
    ├─→ /speckit.plan command (Direct Read)
    │   └─→ specs/*/plan.md (Contains Constitution Check section)
    │
    └─→ /speckit.analyze command (Direct Read)
        └─→ Consistency Report (Contains constitution alignment issues)

Note: The following commands do NOT directly read constitution:
- /speckit.specify
- /speckit.clarify
- /speckit.tasks
- /speckit.checklist
- /speckit.implement
- /speckit.taskstoissues
```

## Best Practices

### 1. Use `/speckit.constitution` to Modify Constitution

```bash
# Recommended: Use command to modify constitution, automatically triggers consistency propagation
/speckit.constitution Add new security principle
```

### 2. Run Analysis After Constitution Update

```bash
# After updating constitution, check existing artifacts
/speckit.analyze
```

### 3. Regenerate Affected Plans

```bash
# If analysis finds constitution violations, regenerate plan
/speckit.plan
```

### 4. Record Changes in Constitution

Maintain revision history in constitution file:

```markdown
**Version**: 2.1.0 | **Approved**: 2025-06-13 | **Last Revised**: 2025-07-16
```

### 5. Verify Consistency in PRs

```markdown
## PR Checklist

- [ ] Run /speckit.analyze to check constitution alignment
- [ ] Plan passes all constitution gates
- [ ] No CRITICAL level constitution violations
```

## Limitations and Considerations

### Current Limitations

1. **No automated propagation**: Constitution updates do not automatically update existing artifacts
2. **No automatic notifications**: Constitution updates do not automatically notify developers
3. **No git hooks**: No git hooks to automatically trigger consistency checks
4. **Partial command coverage**: `/speckit.specify`, `/speckit.clarify`, etc. do not directly read constitution

### Considerations

1. **Manual verification is required**: After constitution updates, manually run `/speckit.analyze`
2. **Existing artifacts need manual updates**: After constitution updates, manually regenerate affected artifacts
3. **Constitution conflicts are CRITICAL**: `/speckit.analyze` marks constitution conflicts as CRITICAL severity

## Summary

Spec Kit's consistency propagation mechanism is **command-execution-based**:

- ✅ **`/speckit.constitution`** is the core command for consistency propagation, automatically propagates to templates when updating constitution
- ✅ `/speckit.plan` reads constitution and applies gates when executed
- ✅ `/speckit.analyze` reads constitution and checks consistency when executed
- ❌ No automated constitution change detection (requires manual command execution)
- ❌ Existing spec/plan/tasks artifacts are not automatically updated

**Recommended Workflow**:

1. Use `/speckit.constitution` to modify constitution (automatically triggers consistency propagation to templates)
2. Run `/speckit.analyze` to check if existing artifacts comply with new constitution
3. Based on analysis results, use `/speckit.plan` and `/speckit.tasks` to regenerate affected artifacts
