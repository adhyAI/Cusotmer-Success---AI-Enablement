---
name: meeting-notes-highlights
description: >
  Meeting notes and transcript intelligence skill. Trigger when the user pastes or uploads meeting notes, call recordings, session summaries, Zoom/Teams transcripts, standup notes, exec briefings, or any raw meeting content — and wants structured highlights, decisions, action items, open questions, or themes extracted. Also trigger on: "process these notes," "what were the key takeaways," "extract action items," "what was decided," "summarize this meeting," "highlight builder," "pull the decisions from this," "who owns what," "what did we agree to," or "add this to the dashboard." Activate for any unstructured conversational input that needs to become structured intelligence.
---

# Meeting Notes Highlights Builder

## Purpose
Transform raw meeting notes, transcripts, or session recordings into structured intelligence: decisions, action items, open questions, themes, and stakeholder signals — with confidence scoring and automatic routing to the dashboard. Designed for PMs, CS leads, and RevOps who sit in a lot of meetings and need to turn conversation into trackable artifacts without losing the nuance.

---

## Step D — Input Detection & Routing (ALWAYS FIRST)

### D1. Classify the Input

| Input Type | Signal | Route to |
|---|---|---|
| **Transcript** (verbatim) | Speaker labels, timestamps, full dialogue | Steps 1 → 2 → 3 → 4 |
| **Meeting notes** (summary) | Bullet points, structured notes, agenda format | Steps 1 → 2 → 3 |
| **Stakeholder session** | Interview, 1:1, user research call | Steps 1 → 2 → 3 → 5 (stakeholder signals) |
| **Decision meeting** | Weekly sync, sprint planning, exec review | Steps 1 → 2 (focus on decisions) |
| **Brainstorm** | Ideation session, whiteboard review | Steps 1 → 3 (focus on themes + ideas) |
| **Status update** | Standup, check-in, project sync | Steps 1 → 2 (focus on blockers + actions) |

### D2. Emit Status Card
```
Meeting Notes Status
Input: [meeting type detected] · [~N participants] · [estimated duration or note length]
Action: [steps running]
Next: [what the output can be used for]
```

---

## Step 1 — Structural Extraction

### 1A. Meeting Metadata
Extract or infer:
- **Meeting type**: 1:1 | Team sync | Stakeholder interview | Exec review | Working session | External call
- **Participants**: Name + role for each speaker/attendee
- **Date**: Extract from content or prompt user
- **Duration**: Infer from timestamp range or note density
- **Context**: What was the meeting about? (one sentence)
- **Outcome**: Did it produce a decision, an alignment, a discovery, or an open question?

### 1B. Hard Extractions (deterministic signals)
Pull these with high confidence — they are stated explicitly:
- **Decisions made**: "We decided to..." / "We're going with..." / "The call is..." / agreed-upon outcomes
- **Action items**: Task + owner + due date. Flag if owner or due date is missing.
- **Commitments**: "I'll..." / "We'll send..." / "By [date]..." — attributed to a person
- **Numbers**: Any metric, figure, date, count, or dollar amount mentioned
- **Named artifacts**: Documents, tools, features, projects, or processes referenced by name

### 1C. Output — Structured Extract
```markdown
## Structured Extract — [Meeting Name, Date]

**Type:** [meeting type]
**Participants:** [name (role), ...]
**One-line outcome:** [what the meeting produced]

### Decisions
| Decision | Made By | Rationale | Confidence |
|---|---|---|---|
| [decision text] | [person] | [why, if stated] | High/Med/Low |

### Action Items
| # | Task | Owner | Due | Status |
|---|---|---|---|---|
| 1 | [task] | [owner] | [date or "not specified"] | Open |

### Commitments
- [Person]: "[commitment text]" — [date if mentioned]
```

---

## Step 2 — Intelligence Extraction

### 2A. Sentiment & Tone
For each participant (if transcript) or the meeting overall (if notes):
- **Sentiment**: Positive / Neutral / Negative / Mixed
- **Emotional markers**: Urgency, Frustration, Enthusiasm, Skepticism, Confusion, Relief
- **Energy inflection points**: What triggered a shift in tone?
- **What was NOT said**: Topics that were conspicuously absent, hedged, or redirected

### 2B. Pain & Friction Signals
Extract any signal of friction, pain, or unmet need — even if unstated directly:
- **Direct pain**: Explicitly named problem ("we waste hours on X")
- **Workaround signal**: "We usually just..." / "The hack we use is..." / "We export it manually..."
- **Repeated mention**: Same problem mentioned >1 time or by >1 person
- **Emotional intensity**: Frustration, resignation, or strong language = high-signal pain

Signal scoring:
| Signal Type | Strength | Action |
|---|---|---|
| Direct + emotional + repeated | **Strong** | Extract as confirmed pain point |
| Direct + specific but once | **Medium** | Extract with "needs validation" flag |
| Indirect / implied / hedged | **Weak** | Note but do not elevate without corroboration |

### 2C. Decision Quality Assessment
For each decision, evaluate:
- **Information completeness**: Was the decision made with enough data?
- **Alignment signal**: Did everyone agree, or were there unresolved reservations?
- **Risk flags**: Was the risk acknowledged and accepted, or was it ignored?
- **Reversibility**: Is this a one-way door (hard to undo) or two-way door?

Flag: `[RISK]` for one-way door decisions made without explicit risk acknowledgment.

---

## Step 3 — Theme & Pattern Identification

### 3A. Thematic Clustering
Group signals (pain points, ideas, questions, concerns) into themes:
- Minimum 2 signals to form a theme
- Label the theme as a noun phrase: "Handover Quality Gap," "Reporting Automation," "Tool Fragmentation"
- Note which participants contributed to each theme

### 3B. Cross-Meeting Pattern Detection
If previous meeting notes exist in context:
- **Recurring themes**: Same pain or topic appearing across multiple meetings
- **Unresolved items**: Action items from previous meetings not yet resolved
- **Drift detection**: Has the team's position on a topic shifted since last discussion?

### 3C. Output — Theme Summary
```markdown
### Themes Identified

**[Theme Label]** — [Confirmed / Emerging]
- Evidence: [2–3 bullet quotes or paraphrased signals]
- Contributors: [participant names]
- Frequency: [first mention / recurring / persistent]
- Recommended action: [validate in next interview | escalate | add to roadmap | monitor]
```

---

## Step 4 — Stakeholder Signals (for interview/research sessions)

Run this step only for 1:1s, stakeholder interviews, or user research sessions.

### 4A. JTBD Extraction
"When [situation], I want [motivation], so I can [outcome]"
Extract or construct from context. Flag confidence level.

### 4B. Segment Update
Based on this session, does the stakeholder's segment or sentiment need updating?
- Champion / Pragmatist / Skeptic / Uninvolved
- Influence: High / Med / Low
- Pain intensity: High / Med / Low

### 4C. Dashboard Update Block
```javascript
// iv[N] — add to INIT_INTERVIEWS in dashboard HTML
{
  id: "iv[N]",
  stakeholderId: "[match to INIT_STAKEHOLDERS id]",
  source: "transcript" | "notes" | "meeting",
  date: "[YYYY-MM-DD]",
  sentiment: [−2 to +2],
  urgency: [1–5],
  summary: "[2–4 sentence summary]",
  quotes: ["[verbatim or close paraphrase]", ...],
  themes: ["[theme label]", ...],
  frictionAreas: ["[area]", ...],
  actionItems: ["[task]", ...]
}
```

---

## Step 5 — Highlights Report

### 5A. One-Page Highlights Document
```markdown
# Meeting Highlights — [Title] · [Date]

## TL;DR
[2–3 sentences: what happened, what was decided, what's next]

## Key Takeaways
1. [most important signal or decision]
2. [second most important]
3. [third]

## Decisions Made
[decision table from Step 1C]

## Action Items
[action items table from Step 1C]

## Open Questions
- [question] — **Owner**: [person or "unassigned"] — **Due**: [date or "no deadline set"]

## Watch List
- [anything flagged as risk, unresolved conflict, or weak-signal concern]

## Themes Updated
- [list any themes that were confirmed, updated, or newly identified]
```

### 5B. Slack/Email Summary Variant (optional)
A shorter version for async sharing:
```
Quick sync summary — [Meeting name, date]

Decisions: [bullet list, 1 line each]
Action items: [bullet list with owner initials]
Blockers: [any flagged risks or blockers]
Next meeting: [date/time if mentioned]
```

---

## Quality Standards

| Check | Requirement |
|---|---|
| **Action item completeness** | Every action item has an owner; flag if missing |
| **Decision confidence** | Risk-flag any one-way-door decision made without explicit risk acknowledgment |
| **Signal discipline** | Weak signals labeled and not elevated without corroboration |
| **JTBD format** | All JTBD statements follow "When / I want / So I can" structure |
| **Stakeholder attribution** | All quotes and signals attributed to a specific person or role |
| **Dashboard compatibility** | Any interview or stakeholder data includes a dashboard update block |

---

## Dashboard Integration

This skill feeds:
- **Highlights tab** → `INIT_THEMES` (new or updated theme records)
- **Stakeholders tab** → `INIT_INTERVIEWS` (new interview records per session)
- **Project Status tab** → scheduling status updates if meeting was a scheduled interview
- **Results tab** → `INIT_RESULTS` decisions from meeting

Data model in `outputs/cs-framework-research-dashboard.html`.
