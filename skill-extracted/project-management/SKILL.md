---
name: project-management
description: >
  Project management and execution tracking skill. Trigger when the user wants to track project status, manage milestones, surface blockers, run a retrospective, update stakeholders, create a project brief, assign ownership, run a sprint review, plan a quarter, or keep a project on track. Also trigger on: "what's the status," "are we on track," "what are the blockers," "update the project plan," "plan this quarter," "create a project brief," "write a status update," "retro," "who owns what," "what's slipping," "sprint planning," "track progress," "project health check," or "exec update." Activate for any work that involves coordinating multiple people, workstreams, milestones, or timelines toward a defined outcome.
---

# Project Management Skill

## Purpose
Turn messy project state into clear status, surfaced blockers, actionable next steps, and stakeholder-ready updates. Covers the full project lifecycle: brief → milestones → execution tracking → blocker resolution → retrospective. Designed for PMs and CS leads running cross-functional initiatives with multiple moving parts.

---

## Step A — Project State Detection

### A1. Classify the Ask

| Ask Type | Signal | Route to |
|---|---|---|
| **New project** | "Start a project," "plan this initiative," no prior context | Steps 1–2 (Brief + Milestones) |
| **Status check** | "What's the status," "are we on track" | Step 3 (Status Report) |
| **Blocker surface** | "What's blocking us," "we're stuck on X" | Step 4 (Blocker Analysis) |
| **Stakeholder update** | "Write an exec update," "status update for [person]" | Step 5 (Comms) |
| **Retrospective** | "Retro," "what went well/poorly," "project closed" | Step 6 (Retro) |
| **Sprint/iteration** | "Sprint planning," "sprint review," "what's in scope" | Step 7 (Iteration) |

### A2. Status Card
```
Project Status
Project: [name]
Phase: [Brief / Planning / In Progress / At Risk / Complete]
Health: [Green / Yellow / Red]
Last updated: [date]
Next action: [most critical thing to do right now]
```

Health definitions:
- **Green**: On track, no material blockers, team aligned
- **Yellow**: At risk — one significant blocker, scope creep, or alignment gap
- **Red**: Off track — milestone slipped, critical blocker unresolved, or stakeholder misalignment

---

## Step 1 — Project Brief

### 1A. Define the Problem Being Solved
- **Problem statement**: What situation are we fixing? Why does it matter now?
- **Decision this unblocks**: What can the business do after this project that it can't do today?
- **Non-goals**: What are we explicitly NOT doing? (prevents scope creep)

### 1B. Outcome Definition
- **Primary outcome**: The one measurable change that means this project succeeded
- **Leading indicator**: What can we measure during execution to know we're on track?
- **Success criteria**: Specific, time-bound, and falsifiable

```markdown
## Project Brief — [Name]

**Problem:** [1–2 sentences]
**Why now:** [urgency / opportunity / deadline driver]
**Outcome:** [what's different when this is done]
**Success metric:** [specific, measurable, time-bound]
**Non-goals:** [explicit exclusions]
**Owner:** [single accountable person]
**Timeline:** [start → milestones → ship date]
**Stakeholders:** [who needs to know / approve / use the output]
```

### 1C. RACI for Cross-Functional Work
| Task | Responsible | Accountable | Consulted | Informed |
|---|---|---|---|---|
| [key task] | | | | |

Rules:
- One person Accountable per task (not two)
- If "Responsible" and "Accountable" are the same person, that's fine — if they're different people, the Responsible person needs a clear handoff protocol

---

## Step 2 — Milestone Planning

### 2A. Break Down the Work
Milestones should be:
- **Outcomes, not activities**: "Beta shipped to 3 users" not "Development started"
- **Verifiable**: Someone can objectively confirm this happened
- **Spaced appropriately**: No more than 2 weeks between checkpoints for in-flight work

### 2B. Dependency Mapping
For each milestone:
- What must be true **before** this can start?
- What does this **unlock** when complete?
- Who must **approve or sign off** before moving to the next milestone?

### 2C. Milestone Table
```markdown
## Milestones

| # | Milestone | Owner | Target Date | Dependencies | Status |
|---|---|---|---|---|---|
| M1 | [outcome-based description] | [name] | [YYYY-MM-DD] | [none or M-N] | Not Started |
```

### 2D. Critical Path
Identify the sequence of milestones where a slip in any one cascades to the ship date. Mark these **[CRITICAL PATH]** in the table.

---

## Step 3 — Status Reporting

### 3A. Current State Snapshot

| Dimension | Status | Notes |
|---|---|---|
| **Schedule** | On track / At risk / Slipped | [delta from plan] |
| **Scope** | Stable / Expanding / Reduced | [what changed] |
| **Resources** | Adequate / Stretched / Blocked | [constraint] |
| **Quality** | Meets bar / Below bar / Unknown | [evidence] |
| **Stakeholder alignment** | Aligned / Diverging / Unknown | [last checked] |

### 3B. Milestone Status Update
```markdown
## Status Update — [Project Name] · [Date]

**Overall health:** [Green / Yellow / Red]
**One-line summary:** [what's happening right now in one sentence]

### Milestone Progress
| Milestone | Owner | Due | Status | Notes |
|---|---|---|---|---|
| M1 | | | ✓ Done | |
| M2 | | | In Progress | [% complete or what's happening] |
| M3 | | | Not Started | [blocked by / waiting for] |

### Blockers
[See Step 4 output]

### What's Next
1. [most critical next action, owner, date]
2. ...
```

---

## Step 4 — Blocker Analysis

### 4A. Blocker Classification

| Type | Definition | Response |
|---|---|---|
| **Hard blocker** | Cannot proceed without resolution | Escalate immediately, assign unblocking owner |
| **Soft blocker** | Slows progress but work-aroundable | Find the workaround, track the underlying issue |
| **Decision blocker** | Waiting for a decision from a stakeholder | Write the decision brief, set a deadline |
| **Information blocker** | Missing data or access | Identify the source, assign to get it |
| **Resource blocker** | Missing capacity or tooling | Escalate or descope |

### 4B. Blocker Record
For each blocker:
```
Blocker: [what's blocked]
Type: [Hard / Soft / Decision / Information / Resource]
Blocking since: [date]
Impact: [what slips or fails if unresolved]
Unblocking owner: [name]
Unblocking action: [specific next step]
Target resolution: [date]
Escalation needed: [Yes / No — if Yes: who, by when]
```

### 4C. Blocker Escalation Brief
When a blocker requires escalation, generate:
```
To: [decision-maker name]
Re: Blocker on [project] — needs your input

Context: [2 sentences on what the project is]
Blocker: [what's blocked and why it's blocked]
Impact: [what slips if unresolved — date/outcome]
Options:
  A. [option] — [tradeoff]
  B. [option] — [tradeoff]
Recommended: [A or B] because [reason]
Decision needed by: [date]
```

---

## Step 5 — Stakeholder Communications

### 5A. Audience-Calibrated Updates
| Audience | Format | Frequency | What They Need |
|---|---|---|---|
| **Executive sponsor** | 3–5 bullet exec summary | Weekly or at milestones | Health, key decisions, risks — not details |
| **Core team** | Full status report | Weekly | Everything: milestones, blockers, next actions |
| **Informed stakeholders** | Brief async update | At milestones | What shipped, what's next, what we need from them |
| **External partners** | Formal update | At agreed cadence | Scope, dates, dependencies — no internal friction |

### 5B. Exec Summary Format
```markdown
[Project Name] — Week of [date]

Status: [Green / Yellow / Red]

Progress this week:
- [what shipped or moved]
- [decision made]

Upcoming:
- [next milestone and date]

Risks / decisions needed:
- [if any — keep to 1–2 max, with clear ask]
```

### 5C. Stakeholder Alignment Check
Before any major milestone, confirm:
- Does the executive sponsor know the health status?
- Do all RACI-Accountable people agree on what's in scope?
- Are there any silent dissenters who haven't been heard?

---

## Step 6 — Retrospective

### 6A. Three-Column Retro
| What went well | What didn't | What to change |
|---|---|---|
| [keep doing] | [stop doing] | [start doing or do differently] |

### 6B. Five-Category Deep Retro (for complex projects)
- **Process**: Did our workflow support the work?
- **Communication**: Were the right people informed at the right time?
- **Scope management**: Did we control scope well?
- **Estimation accuracy**: How close were our time/effort estimates?
- **Team health**: Did the team feel supported and productive?

### 6C. Learning Capture
For each retro finding:
- Is this a one-time issue or a systemic pattern?
- Does it warrant a process change for future projects?
- Who needs to know about this finding outside the immediate team?

---

## Step 7 — Sprint / Iteration Management

### 7A. Sprint Planning Checklist
Before committing to a sprint:
- [ ] All items are scoped to fit in the sprint (no "just start it this sprint" items)
- [ ] Each item has an acceptance criterion
- [ ] Dependencies within the sprint are identified
- [ ] Capacity accounts for meetings, reviews, and interrupts (~20–30%)

### 7B. Daily Standup Structure
Three questions — one sentence each:
1. What did I complete since last standup?
2. What am I working on next?
3. Is there anything blocking me?

Flag: if anyone says "still working on X" for two standups in a row → surface to PM immediately.

### 7C. Sprint Review Format
```markdown
## Sprint [N] Review — [dates]

### Completed
- [item] — [acceptance criteria met: yes]

### Incomplete (carried forward)
- [item] — [reason not completed]

### Blocked / Deferred
- [item] — [blocker or decision needed]

### Velocity
- Planned: [N points/items]
- Completed: [N points/items]
- Note: [any context on variance]
```

---

## Quality Standards

| Check | Requirement |
|---|---|
| **Single accountable owner** | Every milestone and blocker has exactly one owner |
| **Outcome-based milestones** | Milestones are outcomes, not activities |
| **Blocker urgency** | Every hard blocker has an unblocking owner and a target resolution date |
| **Exec updates** | ≤5 bullets, health status first, decision ask explicit if needed |
| **Retro actionability** | Every retro learning has a "who acts on this" answer |
| **RICE/prioritization alignment** | Project milestones connect back to the prioritized opportunity they implement |

---

## Dashboard Integration

This skill feeds:
- **Project Status tab** → Coverage, scheduling board, next actions, pipeline health
- **Results tab** → Decisions made, roadmap items, outcomes being tracked

Update next-actions list in `SectionStatus` and roadmap items in `SectionResults` as the project progresses.
