# User Research Pipeline — Automation Spec

## Overview

The pipeline is a stateful, auto-routing system that operates on top of the skill's analytical steps. Its job is to eliminate the manual stitching between research activities — you drop in data (or a request), and the pipeline routes it to the right step, generates the right output, and updates the dashboard automatically.

This document is the reference for how the pipeline works. Use it when integrating the skill into a workflow tool, extending it with new integrations, or debugging unexpected routing behavior.

---

## Pipeline Stages

The research lifecycle moves through 6 stages. The pipeline detects the current stage and routes accordingly.

```
┌──────────────┐     ┌──────────────┐     ┌──────────────────┐
│  Blank Slate │────▶│   Planning   │────▶│  Data Collection │
└──────────────┘     └──────────────┘     └──────────────────┘
                                                    │
                            ┌───────────────────────┘
                            ▼
                     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
                     │  Synthesis   │────▶│  Reporting   │────▶│   Complete   │
                     └──────────────┘     └──────────────┘     └──────────────┘
```

| Stage | Entry Condition | Key Outputs | Exit Condition |
|---|---|---|---|
| **Blank Slate** | Nothing shared | Research goals, stakeholder list, interview guides, scheduling pack | Stakeholder list created |
| **Planning** | Stakeholder list exists | Segmentation (Step B), scheduling pack (Step E), interview priority queue | First interview scheduled |
| **Data Collection** | Interviews being conducted | Per-interview analysis (Steps 0–3), sentiment records (Step D), dashboard Tab 1/2 updates | ≥50% of interviews complete OR themes stabilizing |
| **Synthesis** | Themes identified, ≥3 interviews done | Problem statements (Step C), friction heatmap (Step D), opportunity map (Step 4), roadmap (Step 5) | All themes have confidence ≥3 |
| **Reporting** | Research complete | Stakeholder report (Step 6), final dashboard (Step 7) | Report delivered |
| **Complete** | Report delivered, team actioned | Improvement audit (Step 8), archive | — |

---

## Input Types & Auto-routing

When data is dropped in, the pipeline classifies it and routes to the correct step:

| Input Pattern | Detected As | Routes To |
|---|---|---|
| Long-form text with first-person quotes | Interview transcript | Steps 0 → 1 → 2 → 3 → D → Dashboard update |
| Comma/newline-separated short responses | Survey responses | Step 1B (aggregate) → Step 3 → Dashboard Tab 2 |
| Bullet points with action items and decisions | Meeting notes | Step 1B (decisions/blockers) → Open Questions update |
| "User said X, then Y" or session summary | Session summary | Steps 1 → 2 → D |
| Stakeholder names + roles (table or list) | Stakeholder list | Step B (segmentation) → Step E (scheduling) |
| Research goals or problem description | Research brief | Step P (planning) → Step P3 (guides) |
| "Schedule X," "reach out to Y" | Scheduling request | Step E |
| "What's the real problem with X" | Problem framing request | Step C |
| "Where's the friction" or "how do people feel" | Analysis request | Step D → Dashboard Tab 8 |

### Ambiguous Input
If input doesn't clearly match any pattern, ask one clarifying question:
> "Is this an interview transcript, meeting notes, a stakeholder list, or something else? I'll route it correctly once I know."

---

## Automation Triggers

These conditions fire steps automatically without being asked:

| Trigger | Fires |
|---|---|
| Any new interview transcript added | Step 1 → Step 2 → Step D → Dashboard Tab 1/8 update |
| ≥3 interviews complete | Step 3 (theming) if not yet run; Step C (problem statements) |
| New theme confirmed | Step C (problem statement for that theme) |
| Any theme reaches confidence ≥4 | Step 4 (opportunity mapping for that theme) |
| All Tier 1 + Tier 2 interviews complete | Step 5 (roadmap), Step 6 (report prompt) |
| Stakeholder added to list | Step B (re-run segmentation), Step E (generate scheduling pack) |
| 5+ days since last outreach with no response | Flag in Tab 9 Next Actions: "Follow up with [Name]" |
| Coverage gap detected (tier with 0 interviews) | Tab 9 alert + Step E scheduling nudge |

---

## State Management

The pipeline maintains this state object throughout the session. Each step reads from and writes to it.

```javascript
const pipelineState = {
  // Stage tracking
  stage: "Blank Slate",  // current pipeline stage
  lastUpdated: "2026-04-15",

  // Stakeholder tracking
  stakeholders: [],        // array of stakeholder objects (from data model)
  schedulingQueue: [],     // ordered list of stakeholder IDs by priority

  // Interview tracking
  interviewsDone: 0,
  interviewsTotal: 0,      // set from stakeholder list
  lastSynthesisAfter: 0,   // interview count at last synthesis run

  // Analysis outputs
  themes: [],
  problemStatements: [],
  opportunities: [],
  roadmapItems: [],
  openQuestions: [],

  // Sentiment/friction
  sentimentRecords: [],
  frictionHeatmap: [],

  // Scheduling
  schedulingPack: [],

  // Coverage
  coverageByTier: { 1: 0, 2: 0, 3: 0, 4: 0 },
  coverageGaps: [],

  // Health
  healthScore: 0,
  dashboardVersion: 0
};
```

---

## Output Artifacts Per Stage

Each stage produces a defined set of artifacts. This table maps stage → what the PM should have at the end.

| Stage | Artifacts Produced |
|---|---|
| **Blank Slate** | Research goals doc, Stakeholder list (Tabs 1, 5), Interview guides (templates), Scheduling pack (Tab 9 center) |
| **Planning** | Segmentation 2×2 matrix (Tab 1), Priority queue (Tab 9), Scheduling pack with email/calendar/Slack per stakeholder |
| **Data Collection** | Per-interview analysis summaries, Dashboard Tabs 1 + 2 (stakeholders + themes), Sentiment records (Tab 8 bottom) |
| **Synthesis** | Problem statements (Tab 7), Friction heatmap (Tab 8 top), Opportunity map (Tab 3), Roadmap draft (Tab 4) |
| **Reporting** | Full stakeholder report (Step 6 output), Final 9-tab dashboard, Open questions resolved list |
| **Complete** | Research improvement audit, Coverage analysis, Archive-ready dashboard |

---

## Integration Hooks

The pipeline is designed to accept inputs from common PM and research tools. When integrating, format the data as the input type below and drop it into the conversation.

| Tool | Export Format | Plug In As |
|---|---|---|
| **Notion** | Page export (markdown) | Interview transcript or meeting notes |
| **Airtable** | CSV export | Survey responses or stakeholder list |
| **Loom / Otter.ai** | Auto-transcript (text) | Interview transcript |
| **Typeform** | Response export (CSV or JSON) | Survey responses |
| **Google Forms** | Spreadsheet export | Survey responses |
| **Calendly** | Booking list (CSV) | Stakeholder scheduling status update |
| **Slack** | Channel export or thread copy | Meeting notes / informal signals |
| **Dovetail / Grain** | Tag export or highlight reel | Synthesized signals (treat as Step 3 input) |
| **Miro / FigJam** | Affinity map export (text) | Theming input (treat as Step 3 in progress) |

### Integration Pattern
1. Export data from the tool in its native format
2. Paste or attach to the conversation with a one-line label: "This is a Loom transcript from my interview with [Name] on [date]."
3. The pipeline auto-routes it and updates the dashboard.

---

## Scheduling Automation Logic

The scheduling system in Step E follows this priority logic:

```
Priority Score = (Tier Weight × Tier Score) + (Influence Score) + (Pain Score) − (Days Since Outreach × 0.1)

Where:
  Tier Weight: Tier 1 = 4, Tier 2 = 3, Tier 3 = 2, Tier 4 = 1
  Influence Score: High = 3, Med = 2, Low = 1
  Pain Score: High = 3, Med = 2, Low = 1
  Days Since Outreach: penalizes for time elapsed (follow-up nudge)
```

Higher score = interview sooner. Re-sort queue whenever:
- A new stakeholder is added
- An interview is completed
- A stakeholder status changes
- A coverage gap is detected

---

## Confidence Calibration

The pipeline tracks confidence scores (1–5) per insight and adjusts them automatically:

| Event | Confidence adjustment |
|---|---|
| Second source independently confirms an insight | +1 (max 5) |
| Third source confirms with behavioral evidence | +1 (max 5) |
| Source contradicts an existing insight | −1 for both (flags contradiction) |
| Insight is secondhand (person heard it from someone else) | −1 |
| Insight is based on observed behavior (not stated preference) | +1 |
| Insight comes from only Tier 4 (skeptics) | No change, but flag as "skeptic-sourced" |

---

## Health Score Calculation

The pipeline health score (0–100) shown in Tab 9 is calculated as:

```
Health Score = 
  (Coverage score / max_coverage × 30) +       // Are all tiers represented?
  (Avg confidence / 5 × 25) +                   // How strong is the signal?
  (Open questions answered / total × 20) +       // How much is still unknown?
  (Themes with problem statements / total × 15) + // Is synthesis complete?
  (Calendar adherence × 10)                      // Are we on schedule?

Calendar adherence = 1 if on track, 0.5 if slightly behind, 0 if significantly behind
```

Thresholds:
- 70–100: Research is on track, strong signal, ready to synthesize or report
- 40–69: Research in progress, some gaps, continue interviews before reporting
- 0–39: Early stage or significant coverage gaps — prioritize scheduling

---

## Error Handling & Edge Cases

| Situation | Pipeline response |
|---|---|
| Only 1 interview done | Don't run theming. Flag: "Single-source — need 2+ interviews before theming." |
| All stakeholders are from one team | Flag selection bias. Add coverage gap alert for other teams. |
| Contradicting insights across stakeholders | Flag contradiction explicitly. Add to Open Questions. Don't average them away. |
| User adds research for a completely new topic | Ask: "Is this a new research stream or continuation of the current one?" |
| Interview date is missing | Extract from context if possible. Otherwise mark as "Date unknown" — don't block analysis. |
| Stakeholder segment can't be inferred | Default to "Pragmatist." Mark as "Segment: Inferred (update me)." |
