# Functional Design

## Purpose
**Detailed business logic design per unit**

Functional Design focuses on:
- Detailed business logic and algorithms for the unit
- Domain models with entities and relationships
- Detailed business rules, validation logic, and constraints
- Technology-agnostic design (no infrastructure concerns)

**Note**: This builds upon high-level component design from Application Design (INCEPTION phase)

## Prerequisites
- Units Generation must be complete
- Unit of work artifacts must be available
- Application Design recommended (provides high-level component structure)
- Execution plan must indicate Functional Design stage should execute

## Overview
Design detailed business logic for the unit, technology-agnostic and focused purely on business functions.

## Steps to Execute

### Step 1: Check Dependencies
- Read unit definition from `aidlc-docs/inception/application-design/unit-of-work.md`
- Read unit dependencies from `aidlc-docs/inception/application-design/unit-of-work-dependency.md`
- Check `aidlc-docs/construction/shared-contracts/` for required contracts
- **IF missing dependencies**: Ask dependency question with smart suggestions

**Dependency Question Format**:
```markdown
"Unit [A] depends on [dependency description] from Unit [B].

**AI-DLC Suggestion**: [Smart suggestion based on dependency type]

How should Unit [A] proceed:
A) Wait for Unit [B] to publish complete contract (RECOMMENDED for [reason])
B) Go discuss with Unit [B] team to get preliminary contract
C) Proceed with assumptions and reconcile later
D) Other (describe approach)

[Answer]: 
```

**Smart Suggestions**:
- **Shared database/domain models** → "Critical dependency - discuss with other team"
- **Large message contracts (20+ fields)** → "Complex contract - coordinate with publishing team"
- **Simple API calls** → "Can likely proceed with stubs"
- **Cross-team dependencies** → "Consider team coordination"

### Step 2: Analyze Unit Context
- Read assigned stories from `aidlc-docs/inception/application-design/unit-of-work-story-map.md`
- Understand unit responsibilities and boundaries
- Load Cross-Unit Design decisions (if available)

### Step 3: Create Functional Design Plan
- Generate plan with checkboxes [] for functional design
- Focus on business logic, domain models, business rules
- Each step should have a checkbox []

### Step 4: Generate Context-Appropriate Questions
**DIRECTIVE**: Thoroughly analyze the unit definition and functional design artifacts to identify ALL areas where clarification would improve the functional design. Be proactive in asking questions to ensure comprehensive understanding.

**CRITICAL**: Default to asking questions when there is ANY ambiguity or missing detail that could affect functional design quality. It's better to ask too many questions than to make incorrect assumptions.

- EMBED questions using [Answer]: tag format
- Focus on ANY ambiguities, missing information, or areas needing clarification
- Generate questions wherever user input would improve functional design decisions
- **When in doubt, ask the question** - overconfidence leads to poor designs

**Question categories to consider** (evaluate ALL categories):
- **Business Logic Modeling** - Ask about core entities, workflows, data transformations, and business processes
- **Domain Model** - Ask about domain concepts, entity relationships, data structures, and business objects
- **Business Rules** - Ask about decision rules, validation logic, constraints, and business policies
- **Data Flow** - Ask about data inputs, outputs, transformations, and persistence requirements
- **Integration Points** - Ask about external system interactions, APIs, and data exchange
- **Error Handling** - Ask about error scenarios, validation failures, and exception handling
- **Business Scenarios** - Ask about edge cases, alternative flows, and complex business situations

### Step 5: Store Plan
- Save as `aidlc-docs/construction/plans/{unit-name}-functional-design-plan.md`
- Include all [Answer]: tags for user input

### Step 6: Collect and Analyze Answers
- Wait for user to complete all [Answer]: tags
- **MANDATORY**: Carefully review ALL responses for vague or ambiguous answers
- **CRITICAL**: Add follow-up questions for ANY unclear responses - do not proceed with ambiguity
- Look for responses like "depends", "maybe", "not sure", "mix of", "somewhere between"
- Create clarification questions file if ANY ambiguities are detected
- **Do not proceed until ALL ambiguities are resolved**

### Step 7: Generate Functional Design Artifacts
- Create `aidlc-docs/construction/{unit-name}/functional-design/business-logic-model.md`
- Create `aidlc-docs/construction/{unit-name}/functional-design/business-rules.md`
- Create `aidlc-docs/construction/{unit-name}/functional-design/domain-entities.md`

### Step 8: Extract and Publish Contracts
- **Extract contracts** from functional design artifacts:
  - API endpoints and schemas (if unit exposes APIs)
  - Message formats (if unit publishes events)
  - Domain models (if unit owns shared entities)
  - Database schemas (if unit owns tables)
- **Publish to shared location**: Create `aidlc-docs/construction/shared-contracts/{unit-name}-contracts.md`

**Contract Template**:
```markdown
# {Unit-Name} Contracts

## API Contracts (if applicable)
### GET /endpoint
- **Request**: {schema}
- **Response**: {schema}

## Message Contracts (if applicable)
### EventName
- **Schema**: {message format}
- **Trigger**: {when published}

## Domain Models (if applicable)
### EntityName
- **Fields**: {field definitions}
- **Relationships**: {related entities}

## Database Schemas (if applicable)
### TableName
- **Columns**: {column definitions}
- **Constraints**: {keys, indexes}
```

### Step 9: Present Completion Message
- Present completion message in this structure:
     1. **Completion Announcement** (mandatory): Always start with this:

```markdown
# 🔧 Functional Design Complete - [unit-name]
```

     2. **AI Summary** (optional): Provide structured bullet-point summary of functional design
        - Format: "Functional design has created [description]:"
        - List key business logic models and entities (bullet points)
        - List business rules and validation logic defined
        - Mention domain model structure and relationships
        - DO NOT include workflow instructions ("please review", "let me know", "proceed to next phase", "before we proceed")
        - Keep factual and content-focused
     3. **Formatted Workflow Message** (mandatory): Always end with this exact format:

```markdown
> **📋 <u>**REVIEW REQUIRED:**</u>**  
> Please examine the functional design artifacts at: `aidlc-docs/construction/[unit-name]/functional-design/`



> **🚀 <u>**WHAT'S NEXT?**</u>**
>
> **You may:**
>
> 🔧 **Request Changes** - Ask for modifications to the functional design based on your review  
> ✅ **Continue to Next Stage** - Approve functional design and proceed to **[next-stage-name]**

---
```

### Step 10: Wait for Explicit Approval
- Do not proceed until the user explicitly approves the functional design
- Approval must be clear and unambiguous
- If user requests changes, update the design and repeat the approval process

### Step 11: Record Approval and Update Progress
- Log approval in audit.md with timestamp
- Record the user's approval response with timestamp
- Mark Functional Design stage complete in aidlc-state.md
