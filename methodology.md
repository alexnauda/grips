# GRIPS Methodology v0.0.2-dev

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
8. **Harness and Model Agnostic**: The methodology is portable and self-sufficient; any harness-specific automation is an optional, separable adapter (see 1.4)
9. **Context Hygiene**: Isolate bounded execution work from the durable control channel so its noise does not accumulate in specs or status (see 5.9)

### 1.3 When to Use GRIPS

**GRIPS is designed for:**
- New projects starting from scratch (v0.0.1 focuses on greenfield development)
- Teams using AI agents for development
- Projects requiring clear specification-to-implementation traceability
- Long-lived projects where documentation drift is a concern

**Not yet supported:**
- Adding GRIPS to existing projects with established codebases (deferred to future versions)

### 1.4 Two-Layer Architecture: Methodology and Adapters

GRIPS is organized in two layers so that it stays portable while still allowing harness-specific automation:

- **Layer 1 — the methodology**: the portable, harness- and model-agnostic specification in this document and a project's specs. It is the single source of truth and must be fully executable by any capable human-and-agent pair with nothing beyond a text editor and version control.
- **Layer 2 — adapters**: optional, harness-specific operationalizations of Layer 1 — for example, a skill that runs a methodology procedure, or a hook that enforces a constraint. Each adapter implements Layer 1; Layer 1 never depends on an adapter.

**Invariant**: removing every adapter leaves a fully working methodology. Adapters accelerate compliance; they do not define it. Adapters live in a clearly separated location (a dedicated directory or a sibling repository) and are never interleaved with the methodology text. A project may adopt any, all, or none of them.

---

## 2. Directory Structure

### 2.1 Project Structure

Every GRIPS project has this structure:

```
project-root/
├── methodology.md              # Points to GRIPS version in ~/.grips/
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
7. **Retire Absorbed Source Material**: A project sometimes starts from a pre-existing informal document (a design doc, brief, or notes file) used as input to the Requirements/Design Q&A process. Once its content is fully represented in the proper layer(s), delete it from the working tree rather than leaving it in place — a stray document duplicating already-migrated content violates Single Source of Truth (1.2) even though nothing is technically wrong with it. Git history preserves it if it's ever needed again.

### 2.3 Version Control

**All specs/ artifacts are version controlled**, including:
- All documents in specs/
- Q&A sessions
- ADRs
- methodology.md (contains version reference)

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

### 4.2 Breaking Down Specs into Implementation Plans

This section provides detailed guidance for agents on converting design specifications into executable implementation plans.

#### 4.2.1 Step Granularity

**What is a Step?**

A step is a single logical unit of work that:
- Accomplishes ONE specific thing (e.g., "Add CoreData Stack", "Implement Readium ePub Rendering")
- Typically touches 1-3 files maximum
- Results in a compilable, testable state
- Maps to one commit (critical for tracking and rollback)

**Good Granularity Examples:**
- "Add CoreData Stack" (creates 2 files, modifies 1 file)
- "Add Settings Entity" (modifies data model, creates manager service)
- "Implement Readium ePub Rendering" (creates 2 views, modifies 1 file)
- "Add Tap Gesture to Extract Word" (modifies 2 existing files)

**Too Granular (Avoid):**
- "Add import statement to file"
- "Create single property"
- "Write one function"

**Too Broad (Avoid):**
- "Implement entire translation system" (should be broken into: create service, wire to UI, add error handling, etc.)
- "Build complete library feature" (should be broken into: create views, add storage, implement import, etc.)

#### 4.2.2 Step Structure and Format

Each step must follow this standardized structure:

**Step Header:**
```markdown
### Step X.Y: [Descriptive Title]
```

**File Context:**
```markdown
**Files Created**:
- path/to/NewFile.ext

**Files Modified**:
- path/to/ExistingFile.ext
```

**Prompt Block:**

The prompt should be formatted as a blockquote or code fence containing:

```markdown
**Prompt**:

> [Main description of what to implement]
>
> CREATE path/to/NewFile.ext:
>
> TASKS:
> 1. [Specific action to take]
> 2. [Another specific action]
> 3. [Include parameter names, values, and specific details]
>
> REQUIREMENTS:
> - [Constraint or rule that MUST be followed]
> - [Technical requirement, e.g., "Use async/await pattern"]
> - [Things that MUST NOT be done]
>
> UPDATE path/to/ExistingFile.ext:
>
> TASKS:
> 1. [Specific modification to make]
>
> BUILD AND VERIFY:
> [Instructions to build and test the implementation]
>
> VERIFICATION CHECKLIST:
> - [ ] [Specific thing to verify]
> - [ ] [Another verifiable item]
> - [ ] [Must be executable without ambiguity]
```

**After Prompt:**
```markdown
**Verification**:
[Brief summary of what should be verified]

**Status**: ✅ Complete (YYYY-MM-DD) | ⏳ Not Started | 🔄 In Progress
```

#### 4.2.3 Research Phases

Include a RESEARCH PHASE section when:
1. There is a **significant unknown** that cannot be resolved without investigation
2. Research must happen **in collaboration with the human** before code can be written
3. The unknown would **block implementation** without prior investigation

**RESEARCH PHASE Structure:**
```markdown
RESEARCH PHASE:
1. [What needs to be investigated]
2. [Starting points: URLs, docs, example code]
3. [What to look for or understand]
4. [Optional: Deliverable document to create]
```

**Examples:**
- Study third-party API documentation (e.g., Readium's Decoration API)
- Investigate data format from external source (e.g., kaikki.org Wiktionary JSON structure)
- Research technical approach for unfamiliar problem domain

**Important:** Research phases are for unknowns requiring investigation and collaboration, not for looking up simple API documentation.

#### 4.2.4 Level of Detail in Tasks

**Be specific when:**
- The detail is critical to implementation (e.g., exact data types: "UUID, required")
- There are specific values that must be used (e.g., "default: 1.0")
- The parameter/property name is part of the design spec
- Ambiguity would lead to wrong implementation choices

**Be general when:**
- The implementation details are obvious from context
- The agent has enough information to make good choices
- Over-specifying would constrain reasonable alternatives

**Rule of thumb:** Include enough detail that the agent doesn't have to guess critical decisions, but trust the agent to handle implementation details.

#### 4.2.5 Organizing Steps into Stages

Use stages to group related steps into logical milestones:

**When to use stages:**
- When a feature or phase has 4+ steps
- When steps naturally cluster into sub-goals
- When there are logical testing checkpoints
- Always use stages for the first phase/MVP (it needs the most structure)

**When NOT to use stages:**
- Small features with 1-3 steps
- Steps are already clearly independent

**Stage Structure:**
```markdown
## Stage N: [Stage Name and Goal]

### Step N.1: [First Step]
[step details]

### Step N.2: [Second Step]
[step details]
```

**Example:**
- Stage 1: Foundation (Get Dependencies Working)
- Stage 2: Core Feature (Implement Main Functionality)
- Stage 3: Polish (Improve UX and Handle Edge Cases)

#### 4.2.6 Organizing Large Implementation Plans

When a phase has many features (8+ features or 1000+ lines total), split into multiple files:

**Directory Structure:**
```
specs/implementation-plan/phase-N-name/
├── index.md                    # Overview, progress table, quick links
├── 01-feature-name.md          # Feature 1
├── 02-feature-name.md          # Feature 2
├── 03-feature-name.md          # Feature 3
└── ...
```

**Index File Contents:**
- Phase overview and goals
- Progress tracking table (all steps across all features)
- Quick links to each feature file
- Current status and notes

**Benefits:**
- Easier navigation (find specific features quickly)
- Smaller files (easier to edit and review)
- Clear feature boundaries
- Better git diffs (changes isolated to specific feature files)
- Scales well for large phases

#### 4.2.7 Step Ordering and Dependencies

**Critical Ordering Principles:**

1. **Each step must be individually verifiable** - After completing a step, the product should build, run, and/or pass tests
2. **Avoid rework** - Consider dependencies and preload foundational work when needed
3. **Balance foundation vs scope creep** - Build necessary foundations without expanding scope beyond phase goals
4. **Manageable and testable scope** - Don't build more features or complexity than needed for current phase
5. **Review step size** - Before finalizing the plan, review each step and break down any that are too large

**Example of Good Ordering:**
- ✅ Step 1.1: Create Project (foundational, verifiable: builds)
- ✅ Step 1.2: Add Dependencies (foundational, verifiable: builds with dependencies)
- ✅ Step 1.3: Add Test Data (needed for next steps, verifiable: data available)
- ✅ Step 2.1: Implement Core Feature (verifiable: feature works)
- ❌ NOT: Step 2.1: Implement Core Feature and Polish UI (too large, should be separate)

**Avoiding Rework:**
- ✅ Build infrastructure BEFORE building features that need it
- ✅ Create base components BEFORE building complex views
- ❌ Build features with temporary solutions, then refactor later

**When to Preload Foundation:**
- Infrastructure needed by multiple features (do it once, early)
- Breaking changes that would affect existing features (do upfront)
- Core architecture patterns (establish early, avoid refactoring)

**When NOT to Preload:**
- Features not needed for current phase goals
- Optimizations that can wait
- "Nice to have" improvements

### 4.3 Context-Dependent Layer Assignment

What belongs in which layer depends on what the product is and what problem it's solving.

**Examples**:
- **CICD configuration**: Requirements (if building a CICD system) vs. Design/Implementation (if building a mobile app)
- **Error handling strategy**: Design (for most products) vs. Requirements (if building an API integration product)
- **JSON structures**: Implementation (usually) vs. Requirements (when integrating with existing external systems that use JSON in a way that's important to state as requirements)

**Rule of thumb**: Ask "Is this a core feature of what we're building?" If yes, it belongs in requirements. If it's about how we build it, it belongs in design or lower.

### 4.4 Data Modeling Standards

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
- **Process**:
  - Agent asks questions directly in chat conversation with human
  - Human answers directly in chat
  - Agent records both question and answer in the Q&A file immediately
  - Ask one question at a time, wait for answer, record it, then proceed to next question
- Continue until all questions are answered and details are clear enough to proceed to drafting
- Document all Q&A in `specs/{layer}/qa/{version}-{topic}.md`
- Update the Q&A file continuously as each question is asked and answered

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

Before passing through any approval gate, the agent must critically review the current spec layer for gaps.

**Agent Responsibilities at Each Gate:**

1. **Carefully review current spec phase materials**
2. **Critically analyze for gaps:**
   - Are there unclear requirements?
   - Are there unresolved design decisions?
   - Are there missing details needed for next stage?
   - Are there contradictions or inconsistencies?
   - Are there untested assumptions?
3. **Recommend additional refinement if gaps found:**
   - Suggest specific areas needing clarification
   - Propose additional Q&A sessions if needed
   - Identify questions that need answers
4. **Do NOT proceed until both agent and human are satisfied:**
   - Agent gives explicit recommendation: "Ready to proceed" or "Needs refinement"
   - Human gives explicit approval to pass through gate
   - If either has concerns, conduct additional review cycles

**Example Gate Review:**
```
Agent: "I've reviewed the Design spec and identified these gaps before
proceeding to Implementation Plan:

1. Section 2.3 mentions 'error recovery strategy' but doesn't specify
   which errors to handle or how
2. The data model shows relationships but not cardinality constraints
3. API authentication approach is mentioned but not defined

Recommendation: Additional Q&A session to resolve these before creating
the implementation plan. Would you like me to start a Q&A session?"
```

**What Gate Reviews Prevent:**
- Starting implementation with unclear requirements
- Building the wrong thing due to design ambiguity
- Rework from discovering gaps mid-implementation
- Wasted time on implementation that doesn't match intent

**Final Approval:**
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

**Purpose**: Document the decision-making process for significant changes through interactive chat conversations

**Process**:
1. Agent asks question directly in chat
2. Human answers directly in chat
3. Agent immediately records both question and answer in the Q&A file
4. Repeat for next question

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

**Q&A sessions are interactive conversations** - the agent asks questions in chat, the human answers in chat, and the agent records both immediately in the Q&A file. Sessions are updated continuously as each question-answer exchange happens. They serve as the permanent record of why decisions were made.

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

### 5.9 Control Channel and Execution Channels

To keep the durable working session clean, GRIPS distinguishes two kinds of agent activity:

- **Control channel**: the main human-with-agent session where decisions are made, Q&A is conducted, approvals are given, status is maintained, and changes are propagated across layers. Its context is long-lived and worth protecting.
- **Execution channels**: bounded work that produces a discrete deliverable — implementing an implementation-plan Step, conducting research, and, case-by-case, drafting. This work generates large volumes of transcript that carry no lasting value once the deliverable exists.

**Isolate execution from the control channel.** Delegate execution to a sub-agent where the harness provides one, or to a separate session otherwise, so the execution transcript never accumulates in the control channel. Only three things return: the deliverable itself, a concise summary, and any decisions that must be recorded in a Q&A session or an ADR. The raw execution transcript does not return.

**Scope**: delegate implementation and research by default — they are the largest sources of context noise and each yields a deliverable that summarizes cleanly. Treat drafting case-by-case: drafting spec prose often benefits from the control channel's accumulated context and close human interaction, so it is frequently better kept in the control channel. This is a recommendation, not a mandate; apply it where it reduces noise without losing signal.

This principle is the structural companion to the PROJECT-STATUS.md discipline in 6.3. Much status and diary litter originates in the session's urge to record what it did; when execution happens in an isolated channel, that urge is satisfied and discarded within the execution channel, and the control channel and status file stay clean by construction.

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

Track overall project progress in `PROJECT-STATUS.md` at project root. The file uses a **closed template**: it permits exactly the sections below and nothing else. Content that does not fit one of these sections does not belong in the file — route it to its home (see Content Routing, below). A closed schema is what keeps the file from accreting narrative; an open-ended "put status here" remit is what invites it.

**Permitted Sections** (the only sections allowed):
1. **Header** — current phase, current stage, and active work (one to two sentences). Forward-looking only.
2. **Layer Synchronization** — which layers are updated, in sync, or pending (e.g., "Requirements updated in commit abc123, design not yet synced").
3. **Outstanding Work** — incomplete items only, with enough context to resume. Where a tracker (such as GitHub issues) exists, reference each item by number and a one-line status; do not restate the tracked item's full description here. Reserve inline resume-context for work too granular or ephemeral to warrant a tracked issue.
4. **Next Steps** — a short ordered list of what comes next. Incomplete items only.
5. **Sessions & Gates** — an index of Q&A and ADR sessions with their status, and approval gates passed. This is state, not narrative: session names and their statuses, never a log of what was accomplished.

**Meta-rule**: These five sections are the whole file. There is no "Recent Activity," changelog, or "what was accomplished" section under any name. A standing heading that invites a running list of completed work regenerates that list no matter how often it is pruned, so the heading itself is not permitted — the legitimate half of such a list (which sessions and gates exist, and their state) lives in Sessions & Gates.

**Strictly Forbidden**:
- Release notes or change logs (what was added in commits, accomplishments)
- Completed work marked as done in "Outstanding Work"
- Completed items in "Next Steps"
- Detailed feature lists or "What's Included" sections
- Temporal narrative history (use git log for history)
- Milestones with detailed descriptions
- Any content that duplicates information available in git history, spec files, or a tracked issue
- Technical findings, root causes, or gotchas discovered during implementation — even ones
  worth remembering long-term. These belong in an ADR in the layer they constrain (see 8.2),
  not in status narrative. If a finding feels durable enough to write a paragraph about, that
  feeling is the signal it belongs in an ADR instead of here.
- Diary or process narrative — feelings, reflections, or how a session went. This belongs in the
  git commit message; GRIPS hosts no journal.

**Content Routing**: Every kind of content an agent is tempted to add has exactly one home. When the urge is to write something that is not forward-looking state, route it there rather than parking it in status:
- Completed work, "what changed," or an accomplishment log → the git commit message.
- Outstanding or incomplete work → the issue tracker, referenced from Outstanding Work as a pointer (number plus a one-line status).
- A durable technical finding, gotcha, or decision → an ADR in the layer it constrains.
- Process or diary narrative → the git commit message; GRIPS hosts no journal.
- Current phase, stage, active work, next steps, layer synchronization, and the session/gate index → PROJECT-STATUS.md itself.

**Key Distinction**:
- ✅ ALLOWED: Incomplete tasks with detailed context (needed for resumability)
- ❌ FORBIDDEN: Completed accomplishments, change logs, "what we did" (that's git history)

**Purpose**: PROJECT-STATUS.md answers "Where are we now and what's next?" It is NOT a changelog, release notes, or project history. Keep it minimal and forward-looking.

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

## Sessions & Gates
- Q&A 0.0.1: Complete
- Q&A 0.0.2: Complete
- Requirements approval gate: Passed

## Outstanding Work

- #42 API authentication strategy — OAuth vs API key vs JWT still undecided; blocks design §3.2

### Wiktionary parser shape (no tracked issue yet)
**What's Needed**: Confirm the JSON structure varies by language before designing the parser
**Context**: Too exploratory to warrant an issue; captured here for resume only
**Tasks**:
- Spot-check three languages' Wiktionary dumps
- If structure varies, open an issue and move this out of status

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
9. **Maintain PROJECT-STATUS.md continuously** - keep it minimal and forward-looking:
   - Update current phase and stage whenever they change
   - Document what's currently being worked on (1-2 sentences)
   - **Outstanding Work**: List incomplete tasks with full context needed to resume
     - Include what needs to be done, why, and critical context
     - This is where detailed task information belongs (for resumability)
     - Where a tracker exists, reference issues by number plus a one-line status — do not restate the issue's body in status
   - **Next Steps**: List upcoming tasks (brief, no completed items)
   - Track layer synchronization status
   - Maintain the **Sessions & Gates** index (Q&A/ADR session statuses and approval gates passed) — state only, never an accomplishment log
   - **DO NOT** include release notes, change logs, or completed accomplishments, or a "Recent Activity" section under any name
   - **DO NOT** mark items as "✅ COMPLETE" in Outstanding Work (remove them when done)
   - **DO NOT** duplicate information available in git history, spec files, or a tracked issue
   - **Key distinction**: Detailed context for INCOMPLETE work (resumability) vs completed work (git history)
10. Follow the step-by-step layer workflow
11. Document all significant decisions in Q&A sessions or ADRs — this includes technical
    constraints or gotchas discovered mid-implementation that will bind future work (e.g. a
    non-obvious registration/wiring requirement, a platform limitation). A durable technical
    finding belongs in an ADR in the layer it constrains, never in PROJECT-STATUS.md
    narrative, even when it feels like useful context worth preserving

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

Projects reference the methodology via a `methodology.md` file:

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

3. Create methodology.md in project root pointing to this file

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

5. Add `@methodology.md` to your project's CLAUDE.md or AGENTS.md

6. Begin with requirements Q&A session!

---

**End of GRIPS Methodology v0.0.2-dev**
