---
name: skill-builder
description: >
  Meta-skill for building new skills and agents. Trigger when the user says "build me a skill," "create a new skill for," "I need an agent that," "make a SKILL.md for," "turn this into a skill," or describes a repeatable workflow they want automated. Also trigger when the user says "add this to my toolkit," "I keep doing X manually," or asks for a structured intelligence layer for any PM, ops, or analysis task. Produces complete, immediately-usable SKILL.md files in the pm-skills format.
---

# Skill Builder — Meta-Skill

## Purpose
Build new skills from scratch given a workflow description, a problem statement, or an existing process. Output is a complete, deployment-ready SKILL.md file that follows the pm-skills architecture: structured activation triggers, step definitions, routing logic, output formats, and data model spec for any dashboard artifact it feeds.

---

## Step 1 — Intake & Scoping

### 1A. Capture the Workflow
Ask or infer:
- **What is the repeatable task?** One sentence describing what this skill does every time it runs.
- **What triggers it?** What does the user say or paste to activate it?
- **What's the input?** (transcript, document, spreadsheet, stakeholder list, idea, etc.)
- **What's the output?** (structured report, dashboard update, scored list, action items, etc.)
- **Who runs it?** (PM, RevOps, CS lead, analyst — affects tone and format)

### 1B. Identify the Skill Type

| Type | Pattern | Examples |
|---|---|---|
| **Analysis** | Ingests raw data → extracts structured signal | Meeting notes, transcript, survey |
| **Generation** | Ingests context → produces artifact | Proposal, report, roadmap, brief |
| **Scoring** | Ingests candidates → ranks/weights them | Prioritization, risk scoring, opportunity ranking |
| **Tracking** | Maintains running state across sessions | Pipeline, project status, coverage |
| **Transformation** | Converts format or structure | Data cleaning, schema mapping, classification |
| **Orchestration** | Coordinates other skills or steps | Pipeline controller, multi-step workflow |

### 1C. Define the Output Artifact
Every skill must produce at least one concrete output. Specify:
- **Format**: markdown table, structured JSON, React component, narrative report
- **Destination**: dashboard tab, standalone doc, Claude artifact, file
- **Update behavior**: replaces previous / appends / merges by ID

---

## Step 2 — Skill Architecture

### 2A. Write the Activation Block (frontmatter)
```yaml
---
name: [kebab-case-name]
description: >
  [2–3 sentence description covering: what the skill does, what inputs trigger it,
   what outputs it produces, and which domains it covers. Written for a vector
   embedding search — be specific, use the actual vocabulary users would use.]
---
```

Rules for good activation triggers:
- List 8–12 exact phrases a user might say ("analyze this transcript," "what are the themes")
- Include domain vocabulary, not just generic verbs
- Cover both direct commands ("score these opportunities") and natural asks ("what should we prioritize")
- Include "also trigger when" for edge cases

### 2B. Define Steps

Each step follows this structure:
```markdown
## Step [Letter/Number] — [Name] (trigger condition)

### [Letter][number]. [Sub-step Name]
[What this sub-step does, in 2–3 sentences]

**Input:** [what it receives]
**Output:** [what it produces]
**Routing:** [what step runs next, if conditional]
```

Step naming conventions:
- Capital letters (A, B, C...) for orchestration / control flow
- Numbers (1, 2, 3...) for sequential analysis steps
- P prefix for planning steps
- D prefix for detection / routing steps

### 2C. Define the Data Model
If the skill feeds a dashboard, define the data record format:
```javascript
{
  id: "unique-id",
  // required fields
  title: "...",
  status: "...",
  // domain-specific fields
  // ...
}
```

Include:
- All fields needed by dashboard components
- Valid enum values for status/category fields
- Which fields are user-editable vs. Claude-populated

---

## Step 3 — Output Formats

### 3A. Status Card (emit at start of every response)
Every skill emits a status card before any output:
```
[Skill Name] Status
Input: [what was received]
Action: [what step ran]
Output: [what was produced]
Next: [what the user should do next]
```

### 3B. Primary Artifact Format
Define the exact format for the skill's main output. Examples:

**For analysis skills:**
```markdown
## [Section Name]
**Signal strength:** [High/Med/Low]

### Theme: [Label]
- Evidence: [quotes or data points]
- Frequency: [n stakeholders / n sources]
- Confidence: [High/Med/Low]
- Action: [what to do with this]
```

**For scoring skills:**
| # | Item | Score | Rationale | Next Action |
|---|---|---|---|---|

**For generation skills:**
Use the target artifact's native format directly.

### 3C. Dashboard Update Block
If the skill updates the dashboard, output this block so Claude Code can apply it:
```javascript
// INIT_[ARRAY_NAME] update — paste into dashboard HTML
{ id: "...", ... }
```

---

## Step 4 — Quality Checks

Before finalizing any skill output:

### 4A. Signal Calibration
- Flag **weak signal**: single mention, hedged language, speculative source
- Flag **strong signal**: multiple sources, unprompted, specific + emotional
- Never let weak signal drive a high-confidence output

### 4B. Completeness Check
- Has every required field been populated?
- Are there gaps the user needs to fill before the output is usable?
- Is the next action concrete and assigned?

### 4C. Activation Test
Mentally run: "If a user said [trigger phrase], would this skill activate?" Test 3 trigger phrases. If any fail, revise the activation block.

---

## Step 5 — Skill File Generation

Output the complete SKILL.md file as a Claude artifact. Structure:

```
---
name: [name]
description: [activation description]
---

# [Skill Name] — [Type] Skill

## Purpose
[2–3 sentences: what problem it solves, for whom, with what inputs/outputs]

---

## Step A — [Orchestration/Routing]
...

## Step 1 — [First Analysis/Generation Step]
...

## Output Formats
...

## Quality Standards
...

## Dashboard Integration
[Data model + update spec if applicable]
```

---

## Quality Standards for Built Skills

Every skill built by this meta-skill must meet:

| Standard | Requirement |
|---|---|
| **Activation specificity** | 8+ trigger phrases covering natural language + direct commands |
| **Routing clarity** | Every input type has a defined route to a step |
| **Output concreteness** | Output format defined with actual field names + examples |
| **Signal discipline** | Weak vs. strong signal explicitly distinguished |
| **Dashboard integration** | If it produces structured data, a data model is defined |
| **Standalone usability** | The skill file is complete without needing this meta-skill to interpret it |

---

## File Locations

New skills live at:
```
skill-extracted/
├── [skill-name]/
│   ├── SKILL.md          ← primary skill definition (this format)
│   ├── templates/        ← optional: reusable output templates
│   └── references/       ← optional: supporting specs or data models
```

Update `skill-extracted/CLAUDE.md` to reference the new skill and its trigger conditions.
