# GRIPS Methodology v0.0.1

**Guided Requirements Implementation Planning System**

A methodology for spec-driven development with AI agents that maintains living, synchronized documentation across all project layers.

---

## 1. Overview & Principles

### 1.1 Core Concept

GRIPS maintains a living, synchronized hierarchy of project artifacts where changes propagate bidirectionally through all layers. Every change—whether originating at the requirements level, design level, implementation plan, or code—flows through the entire stack to maintain consistency.

This is similar to CASE (Computer-Aided Software Engineering) tools from the 1990s with their CDM ↔ LDM ↔ PDM ↔ physical database synchronization, but adapted for modern agentic development workflows.

### 1.2 Core Principles

1. **Strict Layer Discipline**: All changes must propagate through ALL layers
2. **Single Source of Truth**: Each artifact has one definitive location
3. **No Temporal Cruft**: Documentation stays current; no diary-like release notes mixed with specs
4. **Bidirectional Propagation**: Changes flow up, down, or both ways depending on origin
5. **Agent-Guided Evolution**: Agents draft; humans review and approve
6. **Structured Q&A Process**: All major changes begin with documented Q&A sessions
7. **Phase-Based Implementation**: Work proceeds through prototype → MVP → expansions

### 1.3 When to Use GRIPS

**GRIPS is designed for:**
- New projects starting from scratch (v0.0.1 focuses on greenfield development)
- Teams using AI agents for development
- Projects requiring clear specification-to-implementation traceability
- Long-lived projects where documentation drift is a concern

**Not yet supported:**
- Adding GRIPS to existing projects with established codebases (deferred to future versions)

---

## 2. Directory Structure

### 2.1 Project Structure

Every GRIPS project has this structure:

```
project-root/
├── METHODOLOGY.md              # Points to GRIPS version in ~/.grips/
├── PROJECT-STATUS.md           # Project/phase-level status tracking
├── specs/
│   ├── requirements/
│   │   ├── requirements.md     # Single file initially; splits at 1000 lines
│   │   ├── qa/                 # Q&A sessions by phase
│   │   │   ├── 0.0.1-topic.md
│   │   │   ├── 0.0.2-topic.md
│   │   │   └── 1.0.1-topic.md
│   │   └── adr/                # Architecture Decision Records by phase
│   │       ├── 0.0.1-decision.md
│   │       └── 1.0.1-decision.md
│   ├── design/
│   │   ├── design.md           # Includes architecture
│   │   ├── qa/
│   │   └── adr/
│   └── implementation-plan/
│       ├── implementation-plan.md
│       ├── phase-0-prototype/
│       │   ├── 1-feature.md
│       │   └── 2-feature.md
│       ├── phase-1-mvp/
│       ├── phase-2-expansion/
│       ├── qa/
│       └── adr/
├── src/                        # Code (language-specific location)
└── tests/                      # Tests (language-specific location)
```

**Note**: The `src/` and `tests/` directories shown above are examples only. The actual code structure is completely flexible and should conform to the conventions of your language, platform, and project requirements. For example, a Go project might use different package structures, a Swift project might have Xcode-specific organization, and a Python project might use different module layouts.

### 2.2 File Organization Rules

1. **Start Simple**: Begin with single files (requirements.md, design.md, implementation-plan.md)
2. **Split at 1000 Lines**: When any document exceeds 1000 lines, split it into numbered sections
3. **Numbered Sections**: Use format `1-feature-name.md`, `2-another-feature.md`, etc.
4. **Outline Numbering**: All sections within documents use outline numbering
5. **Phase Versioning**: Use semantic-style versioning for phases (0.0 = prototype, 1.0 = MVP, 2.0+ = expansion)
6. **Q&A and ADR Versioning**: Use three-part versioning (0.0.1, 0.0.2, 1.0.1, etc.) for multiple sessions within a phase

### 2.3 Version Control

**All specs/ artifacts are version controlled**, including:
- All documents in specs/
- Q&A sessions
- ADRs
- METHODOLOGY.md (contains version reference)

Code and tests live in language-specific standard locations and are version controlled in the same repository as specs, often committed together in the same commits.

---

## 3. Terminology Glossary

To ensure consistent understanding across GRIPS projects, these terms have specific meanings:

| Term | Definition |
|------|------------|
| **Phase** | Major project increment with distinct goals (e.g., Phase 0 = Prototype, Phase 1 = MVP, Phase 2+ = Expansion). Uses semantic-style versioning (0.0, 1.0, 2.0). |
| **Layer** | One of the three spec layers: Requirements (what/why), Design (how), or Implementation Plan (steps). |
| **Step** | Granular task in an implementation plan with its own prompt, verification checklist, and status. |
| **Q&A Session** | Documented question-and-answer process for making decisions about spec changes. Versioned using three-part numbers (e.g., 0.0.1, 0.0.2). |
| **Stage** | Workflow stage within GRIPS: Q&A → Draft → Review → Implementation. Not to be confused with project-specific sub-stages. |
| **Approval Gate** | Point where review cycles must complete with approval before proceeding to next layer or stage. |
| **Checkpoint** | Aggregated verification point in implementation plans for testing that requires multiple steps to be complete (e.g., cross-platform testing, integration testing). |
| **Verification Checklist** | Immediate verification items checked after completing each individual implementation step. |
| **Critic Markup** | Standard markdown format for human review feedback on agent-drafted documents. |
| **ADR** | Architecture Decision Record - documents significant architectural or design decisions. |
| **Propagation** | Process of updating all affected layers when a change is made (bidirectional: up, down, or both). |

**Not used in GRIPS**:
- **Sprint**, **Iteration** - GRIPS uses Phases and Stages instead
- **Release** - GRIPS uses "Phase completion" instead

---

## 4. Layer Separation & Concerns

### 4.1 The Three Layers

GRIPS organizes project artifacts into three layers, each with distinct concerns:

#### Requirements Layer (WHAT & WHY)

**Purpose**: Define what the system must do and why

**Contents**:
- Business goals and user needs
- Functional requirements (what the system must do)
- Non-functional requirements (performance, security, scalability, etc.)
- User-facing features and behavior
- External integrations and constraints
- Data models at conceptual level (CDM/LDM - entities and relationships)
- User-facing configuration options (env vars, settings users can control)

**Not Allowed**:
- Implementation details
- Technology choices (except when the technology is a core requirement of the product itself, e.g., "must be an iOS app", "must support iPhone and iPad")
- Code or code-like specifications
- Internal configuration

#### Design Layer (HOW)

**Purpose**: Define how the system will be built

**Contents**:
- System architecture and component breakdown
- Technology choices and frameworks
- Data models at physical level (PDM - schemas, field names, types, but NOT DDL)
- API design and interfaces
- Module/component interactions and dependencies
- Error handling strategies (e.g., "use exponential backoff")
- Testing strategy (what to test, coverage goals)
- Security approach and patterns
- Diagrams and data modeling using analyst-style specifications

**Not Allowed**:
- Actual code implementation
- DDL or other executable code
- Specific function implementations

#### Implementation Plan Layer (STEPS)

**Purpose**: Provide step-by-step executable prompts for agents

**TODO**: Needs expansion - see Q&A session for guidance on breaking down specs into prompt steps

**Contents**:
- Phased breakdown of work (phase-0-prototype, phase-1-mvp, etc.)
- Step-by-step instructions for agents to execute
- Task lists and verification checklists
- Affected file lists (as context/scope, e.g., "Files Created:", "Files Modified:")
- References to specific classes/functions/variables (sparingly - only when necessary for clarity, as these are usually implementation concerns)
- Descriptions of what to implement
- BUILD AND VERIFY sections with concrete verification steps

**Strictly Forbidden**:
- **NO ACTUAL CODE** - only descriptions of what to implement
- Code belongs in the implementation phase, not the plan

### 4.2 Context-Dependent Layer Assignment

What belongs in which layer depends on what the product is and what problem it's solving.

**Examples**:
- **CICD configuration**: Requirements (if building a CICD system) vs. Design/Implementation (if building a mobile app)
- **Error handling strategy**: Design (for most products) vs. Requirements (if building an API integration product)
- **JSON structures**: Implementation (usually) vs. Requirements (when integrating with existing external systems that use JSON in a way that's important to state as requirements)

**Rule of thumb**: Ask "Is this a core feature of what we're building?" If yes, it belongs in requirements. If it's about how we build it, it belongs in design or lower.

### 4.3 Data Modeling Standards

Use established paradigms where applicable:

- **Requirements**: Conceptual Data Model (CDM) and Logical Data Model (LDM) or spec of entities and attributes 
- **Design**: Physical Data Model (PDM) including data types and other details 

**Schemas should be defined analyst-style**:
- Entity names, field names, types
- Relationships and cardinality
- Constraints and validation rules
- NOT as DDL or executable code, which are implementation concerns 

---

## 5. Workflow & Procedures

### 5.1 Workflow Stages

Each phase of a GRIPS project follows a structured workflow with distinct stages:

**TODO**: Needs new section on agent project management and phase guidance - see Q&A session

#### Stage 1: Q&A
- Conduct question-and-answer sessions to understand requirements/design/implementation needs
- **Process**: Ask one question at a time, document the answer, then proceed to the next question
- Continue until all questions are answered and details are clear enough to proceed to drafting
- Document all Q&A in `specs/{layer}/qa/{version}-{topic}.md`
- Update continuously as questions are answered

#### Stage 2: Draft
- Agent drafts specification documents based on Q&A findings
- Create or update requirements.md, design.md, or implementation-plan files
- Start with minimal structure, build out based on Q&A

#### Stage 3: Review Cycles
- Human reviews using Critic Markup
- Agent responds to feedback (resolve simple changes, reply to complex ones)
- Multiple review-revise cycles until human approval
- Each cycle may result in commits

#### Stage 4: Approval Gate
- Human explicitly approves the spec layer
- No changes proceed to next layer without approval
- Approval marks completion of this layer's work

### 5.2 Layer-by-Layer Progression

GRIPS follows a strict sequential workflow for each layer:

```
Requirements Q&A → Draft requirements.md → Review cycles → Approval
    ↓
Design Q&A → Draft design.md → Review cycles → Approval
    ↓
Implementation Plan Q&A → Draft implementation plan → Review cycles → Approval
    ↓
Execute Implementation (write actual code)
```

**Each layer must be approved (pass through the approval gate) before moving to the next.**

### 5.3 Change Propagation

Changes can originate at any layer and must propagate through all affected layers:

**Upward Flow** (Bug/code issue discovered):
```
Code → Implementation Plan → Design → Requirements
```
**Note**: Bug fixes during verification often don't require propagation to higher layers. Only propagate upward when the fix reveals a decision that changes the implementation plan, design, or requirements.

**Downward Flow** (New requirement):
```
Requirements → Design → Implementation Plan → Code
```

**Bidirectional Flow** (Design change):
```
Requirements ← Design → Implementation Plan → Code
```

**Agent Responsibility**: Before starting work, the agent must explicitly identify the change origin layer to determine propagation direction.

**Tracking Propagation**: Document propagation status in PROJECT-STATUS.md under "Layer Synchronization" to track which layers have been updated and which still need updates.

### 5.4 Two-Path Workflow

#### Path 1: Simple/Obvious Changes

When no Q&A session is needed (change is obvious or already clarified):
1. Make code changes and documentation changes together
2. Commit in one commit
3. Done

**Example**: Updating a function signature that's already documented in the implementation plan - update code, update implementation plan reference, commit all together.

#### Path 2: Complex Changes Requiring Spec Updates

When back-and-forth is needed to clarify or flesh out requirements/design:
1. Agent proposes spec change to human and gets confirmation
2. Conduct Q&A session (document in appropriate qa/ directory)
3. Update specs docs (requirements, design, implementation plan)
4. Review cycle(s) with Critic Markup
5. Commit all spec/plan changes
6. Then implement in separate commit(s)

### 5.5 Q&A Sessions

**Purpose**: Document the decision-making process for significant changes

**Location**: `specs/{layer}/qa/{version}-{topic}.md`

**Format**:
```markdown
# {Layer} Q&A Session {version}

**Phase**: {phase version}
**Date Started**: YYYY-MM-DD
**Participants**: {agent name}, {human name}
**Purpose**: {brief description}

---

## Question 1
**Asked by**: {name}
**Question**: {question text}
**Answer**: {answer text}

---

## Question 2
...

---

## Current Understanding
{summary of key decisions and principles}

---

**Last Updated**: YYYY-MM-DD (Question N)
```

**Q&A sessions are updated continuously** as questions are asked and answered. They serve as the permanent record of why decisions were made.

### 5.6 Review Cycles with Critic Markup

**Human reviews using Critic Markup format** to provide feedback on agent-drafted documents.

**Agent response to feedback**:

1. **Simple changes**: If feedback is straightforward and requires no decision-making:
   - Make the change
   - Remove the Critic Markup comment (resolve it)

2. **Complex changes**: If feedback requires decisions or is questionable:
   - Make the requested change
   - Reply to the Critic Markup comment explaining the decision
   - Leave comment in place for human review

### 5.7 Conflict Resolution

When encountering conflicts or inconsistencies:

1. **Clarify with human** unless the change is obviously needed
2. **For simple conflicts**: Proceed with code + docs in one commit
3. **For complex conflicts requiring spec changes**:
   - Confirm with human before starting Q&A process
   - Follow Path 2 workflow (Q&A → spec updates → review → implement)

### 5.8 Commit Strategy

**Spec Review Phase**:
- Multiple commits during review cycles
- One commit per agent change batch
- One commit per human feedback round
- Continue until human approval

**Implementation Phase**:
- One commit per implementation plan Step
- Each commit includes:
  - Code changes for that Step
  - Status updates
  - Any clarifications to specs discovered during implementation
- Alternative: Bug fixes and small tweaks can be separate commits as guided by human

**Commit Message Format**: Follow project conventions (often Conventional Commits)

---

## 6. Implementation Plans & Phasing

### 6.1 Phase Structure

Implementation plans are organized into phases representing project evolution:

- **Phase 0 (0.0)**: Prototype - proof of concept
- **Phase 1 (1.0)**: MVP - minimum viable product
- **Phase 2+ (2.0, 3.0...)**: Expansion - additional features

### 6.2 Implementation Plan Format

Each phase contains numbered feature files with steps:

```markdown
# Feature 1: {Feature Name}

**Part of**: [Phase {X} Prompt Plan](index.md)

---

## Feature 1: {Feature Name}

### Step 1.1: {Step Description}

**Files Created**:
- path/to/NewFile.ext

**Files Modified**:
- path/to/ExistingFile.ext

**Prompt**:

> {Detailed instructions for the agent}
>
> CREATE path/to/NewFile.ext:
>
> TASKS:
> 1. {Task description}
> 2. {Task description}
> 3. {Task description}
>
> REQUIREMENTS:
> - {Requirement}
> - {Requirement}
>
> UPDATE path/to/ExistingFile.ext:
>
> TASKS:
> 1. {Task description}
>
> BUILD AND VERIFY:
> {Build instructions}
>
> VERIFICATION CHECKLIST:
> - [ ] {Verification item}
> - [ ] {Verification item}
> - [ ] {Verification item}

**Verification**:
- {Summary of what should be verified}

**Status**: ✅ Complete (YYYY-MM-DD) | ⏳ In Progress | ⭕ Not Started

---

### Step 1.2: {Next Step}
...

---

## Checkpoint 1: {Checkpoint Name}

**Purpose**: Aggregated verification after completing multiple steps

**Prerequisites**:
- Step 1.1 complete
- Step 1.2 complete

**Verification Tasks**:
- [ ] {Cross-platform testing task}
- [ ] {Integration testing task}
- [ ] {Performance testing task}
- [ ] {End-to-end testing scenario}

**Status**: ✅ Complete (YYYY-MM-DD) | ⏳ In Progress | ⭕ Not Started

---
```

**Checkpoints** are placed after groups of related steps to perform verification that couldn't be done during individual steps (e.g., integration testing, cross-device testing, performance testing).

### 6.3 Status Tracking

GRIPS uses two levels of status tracking:

#### Project/Phase-Level Status (PROJECT-STATUS.md)

Track overall project progress in `PROJECT-STATUS.md` at project root:

**Required Contents**:
- Current phase and stage (e.g., "Phase 0.0, Requirements Review")
- What's currently being worked on
- What's next
- Layer synchronization status (e.g., "Requirements updated in commit abc123, design not yet synced")
- Recent Q&A sessions and their status
- Approval gates passed

**Example**:
```markdown
# Project Status

**Current Phase**: 0.0 (Prototype)
**Current Stage**: Requirements - Review Cycle 2
**Active Work**: Addressing feedback on requirements.md from review

## Layer Synchronization
- Requirements: Updated (commit abc123) - In Review
- Design: Not started
- Implementation Plan: Not started

## Recent Activity
- Q&A 0.0.1: Complete
- Q&A 0.0.2: Complete
- Requirements draft: In review (cycle 2)

## Next Steps
1. Complete requirements review cycle
2. Pass requirements approval gate
3. Begin design Q&A session
```

#### Implementation Step Status (Inline)

**Status is tracked inline** in implementation plan step documents.

**Status must be single source of truth**:
- Each status item has exactly one location (no duplication)
- No diary-like release notes mixed with status
- Release notes are separate documents if needed
- Status tracks completion state, not narrative history

---

## 7. Testing

### 7.1 Testing in GRIPS Layers

**Design Layer**: Specifies what to test
- Testing strategy
- Coverage goals
- Types of tests needed (unit, integration, e2e)

**Implementation Phase**: Contains actual test code
- Tests live in language-specific test directories
- Test details (what they contain, how they operate) are product-specific

**Tests are part of implementation**, not a separate layer.

---

## 8. Rules & Constraints

### 8.1 Mandatory Rules

1. **ALL changes must propagate through ALL layers** - no exceptions
2. **Implementation plans must NEVER include actual code** - only descriptions
3. **Files split at exactly 1000 lines** - no flexibility
4. **All sections use outline numbering**
5. **Version all Q&A and ADR documents** using semantic-style numbering
6. **Q&A sessions must be documented continuously** as they happen
7. **Agent must identify change origin layer** before starting work
8. **No temporal information in specs** - documentation is evergreen
9. **Status is single source of truth** - no mixing with narrative history
10. **All specs/ artifacts are version controlled**
11. **Maintain PROJECT-STATUS.md** - keep project/phase-level status current

### 8.2 Agent Instructions

**When working on a GRIPS project, you MUST**:

1. Read and understand the current specs before making any changes
2. Identify which layer the change originates from
3. Determine propagation direction (up, down, or both)
4. For complex changes, conduct Q&A session first
5. Update all affected layers
6. Accept and use Critic Markup format for reviews
7. Avoid code fences in specs documents except when absolutely necessary (they interfere with markdown editors that support Critic Markup)
8. Never include actual code in implementation plans
9. Keep status tracking current and accurate
10. Follow the step-by-step layer workflow
11. Document all significant decisions in Q&A sessions or ADRs

**When drafting documents**:

1. Start with minimal structure (headings and placeholder text)
2. Build out content based on Q&A findings
3. Use analyst-style specifications, not code
4. Maintain outline numbering throughout
5. Keep documents focused on their layer's concerns
6. Split files when they reach 1000 lines

---

## 9. Examples

### 9.1 Example: Adding a New Feature (Downward Flow)

**Scenario**: User requests "Add user authentication"

**Steps**:

1. **Q&A Session**:
   - Create `specs/requirements/qa/1.0.1-user-authentication.md`
   - Document questions about auth requirements (OAuth vs. local, password policies, etc.)

2. **Update Requirements**:
   - Add authentication requirements to `specs/requirements/requirements.md`
   - Include functional requirements (login, logout, password reset)
   - Include non-functional requirements (security, performance)

3. **Review Cycle**: Human reviews requirements with Critic Markup feedback

4. **Design Q&A Session**:
   - Create `specs/design/qa/1.0.1-user-authentication.md`
   - Discuss technology choices, database schema, API design

5. **Update Design**:
   - Add authentication architecture to `specs/design/design.md`
   - Include data models (User entity with fields)
   - Specify security approach (bcrypt for passwords, JWT for sessions)
   - Define API endpoints

6. **Review Cycle**: Human reviews design

7. **Implementation Plan Q&A**:
   - Create `specs/implementation-plan/qa/1.0.1-user-authentication.md`
   - Break down into steps

8. **Create Implementation Plan**:
   - Create `specs/implementation-plan/phase-1-mvp/3-authentication.md`
   - Step 1.1: Create User model
   - Step 1.2: Add password hashing
   - Step 1.3: Create login endpoint
   - Each with TASKS, REQUIREMENTS, VERIFICATION

9. **Review Cycle**: Human approves implementation plan

10. **Execute**: Agent implements each step, one commit per step

### 9.2 Example: Bug Fix (Upward Flow)

**Scenario**: Error message is incorrect

**Simple Case** (obvious fix):
1. Fix error message in code
2. Update implementation plan to reflect correct message
3. Update design if error handling strategy mentioned
4. Commit all changes together

**Complex Case** (reveals design issue):
1. Clarify with human
2. Human confirms this requires design change
3. Q&A session to understand proper error handling
4. Update design docs
5. Update implementation plan
6. Update requirements if error behavior is a requirement
7. Review cycles
8. Then fix code

### 9.3 Example: File Split at 1000 Lines

**Scenario**: `requirements.md` reaches 1023 lines

**Before**:
```
specs/requirements/
└── requirements.md  (1023 lines)
```

**After**:
```
specs/requirements/
├── requirements.md  (overview + pointers to sections)
├── 1-authentication.md
├── 2-data-management.md
└── 3-reporting.md
```

**requirements.md becomes**:
```markdown
# Requirements

## Overview
{High-level project description}

## Sections

1. [Authentication](1-authentication.md)
2. [Data Management](2-data-management.md)
3. [Reporting](3-reporting.md)
```

---

## 10. GRIPS Methodology Versioning

This document represents GRIPS Methodology version **0.0.1**.

Projects using GRIPS pin to a specific methodology version. Methodology files are stored in:
```
~/.grips/{version}/methodology.md
```

Projects reference the methodology via a `METHODOLOGY.md` file:

```markdown
# GRIPS Methodology

This project uses the GRIPS methodology at version 0.0.1.

For more information, visit: https://github.com/yourorg/grips

@~/.grips/0.0.1/methodology.md
```

**Version Pinning**: Projects stay on their chosen GRIPS version indefinitely. Methodology upgrade paths will be defined in future versions.

---

## 11. Quick Start (Manual Setup for v0.0.1)

**To use GRIPS in a new project**:

1. Create directory structure:
```bash
mkdir -p specs/requirements/qa specs/requirements/adr
mkdir -p specs/design/qa specs/design/adr
mkdir -p specs/implementation-plan/qa specs/implementation-plan/adr
```

2. Create initial template files:

**specs/requirements/requirements.md**:
```markdown
# Requirements

## 1. Overview

{Project description}

## 2. Functional Requirements

{What the system must do}

## 3. Non-Functional Requirements

{Performance, security, scalability, etc.}
```

**specs/design/design.md**:
```markdown
# Design

## 1. Architecture

{System architecture overview}

## 2. Technology Choices

{Frameworks, libraries, tools}

## 3. Data Models

{Entities, schemas, relationships}
```

**specs/implementation-plan/implementation-plan.md**:
```markdown
# Implementation Plan

## Phase 0: Prototype

{Initial proof of concept}

## Phase 1: MVP

{Minimum viable product}
```

3. Create METHODOLOGY.md in project root pointing to this file

4. Create PROJECT-STATUS.md:
```markdown
# Project Status

**Current Phase**: 0.0 (Prototype)
**Current Stage**: Not started
**Active Work**: Setting up GRIPS

## Layer Synchronization
- Requirements: Not started
- Design: Not started
- Implementation Plan: Not started

## Next Steps
1. Begin requirements Q&A session
```

5. Add `@METHODOLOGY.md` to your project's CLAUDE.md or AGENTS.md

6. Begin with requirements Q&A session!

---

**End of GRIPS Methodology v0.0.1**
