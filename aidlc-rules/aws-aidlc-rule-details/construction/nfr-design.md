# NFR Design

## Prerequisites
- NFR Requirements must be complete for the unit
- NFR requirements artifacts must be available
- Execution plan must indicate NFR Design stage should execute

## Overview
Incorporate NFR requirements into unit design using patterns and logical components.

## Steps to Execute

### Step 1: Check Dependencies
- Read unit definition from `aidlc-docs/inception/application-design/unit-of-work.md`
- Read unit dependencies from `aidlc-docs/inception/application-design/unit-of-work-dependency.md`
- Check `aidlc-docs/construction/shared-contracts/` for required contracts
- Check `aidlc-docs/construction/shared-infrastructure/` for shared infrastructure definitions
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
- **Performance/latency requirements** → "Cross-unit performance impacts - recommend coordination"
- **Scalability patterns** → "Can proceed with unit-level patterns and adapter layer"
- **Security patterns** → "Critical dependency - recommend coordination"
- **Resilience patterns** → "Can proceed with circuit breaker/adapter patterns"

### Step 2: Analyze NFR Requirements
- Read NFR requirements from `aidlc-docs/construction/{unit-name}/nfr-requirements/`
- Understand scalability, performance, availability, security needs

### Step 3: Create NFR Design Plan
- Generate plan with checkboxes [] for NFR design
- Focus on design patterns and logical components
- Each step should have a checkbox []

### Step 4: Generate Context-Appropriate Questions
**DIRECTIVE**: Analyze the NFR requirements to generate ONLY questions relevant to THIS specific unit's NFR design. Use the categories below as inspiration, NOT as a mandatory checklist. Skip entire categories if not applicable.

- EMBED questions using [Answer]: tag format
- Focus on ambiguities and missing information specific to this unit
- Generate questions only where user input is needed for pattern and component decisions

**Example question categories** (adapt as needed):
- **Resilience Patterns** - Only if fault tolerance approach needs clarification
- **Scalability Patterns** - Only if scaling mechanisms are unclear
- **Performance Patterns** - Only if performance optimization strategy is ambiguous
- **Security Patterns** - Only if security implementation approach needs input
- **Logical Components** - Only if infrastructure components (queues, caches, etc.) need clarification

### Step 5: Store Plan
- Save as `aidlc-docs/construction/plans/{unit-name}-nfr-design-plan.md`
- Include all [Answer]: tags for user input

### Step 6: Collect and Analyze Answers
- Wait for user to complete all [Answer]: tags
- Review for vague or ambiguous responses
- Add follow-up questions if needed

### Step 7: Generate NFR Design Artifacts
- Create `aidlc-docs/construction/{unit-name}/nfr-design/nfr-design-patterns.md`
- Create `aidlc-docs/construction/{unit-name}/nfr-design/logical-components.md`

### Step 8: Present Completion Message
- Present completion message in this structure:
     1. **Completion Announcement** (mandatory): Always start with this:

```markdown
# 🎨 NFR Design Complete - [unit-name]
```

     2. **AI Summary** (optional): Provide structured bullet-point summary of NFR design
        - Format: "NFR design has incorporated [description]:"
        - List key design patterns implemented (bullet points)
        - List logical components and infrastructure elements
        - Mention resilience, scalability, and performance patterns applied
        - DO NOT include workflow instructions ("please review", "let me know", "proceed to next phase", "before we proceed")
        - Keep factual and content-focused
     3. **Formatted Workflow Message** (mandatory): Always end with this exact format:

```markdown
> **📋 <u>**REVIEW REQUIRED:**</u>**  
> Please examine the NFR design at: `aidlc-docs/construction/[unit-name]/nfr-design/`



> **🚀 <u>**WHAT'S NEXT?**</u>**
>
> **You may:**
>
> 🔧 **Request Changes** - Ask for modifications to the NFR design based on your review  
> ✅ **Continue to Next Stage** - Approve NFR design and proceed to **[next-stage-name]**

---
```

### Step 9: Wait for Explicit Approval
- Do not proceed until the user explicitly approves the NFR design
- Approval must be clear and unambiguous
- If user requests changes, update the design and repeat the approval process

### Step 10: Record Approval and Update Progress
- Log approval in audit.md with timestamp
- Record the user's approval response with timestamp
- Mark NFR Design stage complete in aidlc-state.md
