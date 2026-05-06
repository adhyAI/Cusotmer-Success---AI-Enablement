---
name: user-research-pm
description: >
  Ultimate user research and PM intelligence skill. Trigger whenever the user shares interview notes, transcripts, survey responses, meeting notes, Slack threads, session summaries, or research docs — and wants to extract insights, identify themes, build a roadmap, map use cases, evaluate findings critically, or produce stakeholder reports. Also trigger on: "analyze my interviews," "what patterns are emerging," "build a roadmap from research," "summarize findings," "I interviewed [role] about [topic]," "review this feedback," "start my research," "schedule my interviews," "what are the real problems," or "show me where the friction is." Activate aggressively for any organizational discovery or research work. Also trigger for research dashboards, interview coverage tracking, scheduling, problem statement generation, or research practice improvement. Works across executives, stakeholders, and cross-functional teams.
---

# User Research PM Intelligence Skill

## Purpose
This skill covers the full user research lifecycle — from pipeline orchestration and interview scheduling, to stakeholder segmentation, data analysis, problem statement generation, sentiment/friction mapping, and automated dashboards. The system auto-routes inputs to the right step and maintains a running pipeline state. The primary output is always an **editable, stakeholder-ordered dashboard artifact**.

---

## Step A — Automated Pipeline Orchestration (ALWAYS RUN FIRST)

Run this step at the start of every conversation turn before doing anything else.

### A1. Detect Current Stage
Assess which stage the PM is at based on what's been shared:

| Stage | Signal | Route to |
|---|---|---|
| **Blank slate** | Nothing shared yet | Step P → Step B → Step E |
| **Planning** | Stakeholder list or research goals shared | Step B → Step E → Step P3/P4 |
| **Data collection** | Transcripts, notes, or surveys shared | Steps 0–3 → Step D → Dashboard update |
| **Mid-synthesis** | Themes mentioned, more data coming | Steps 2–3 → Step C → Dashboard Tab 2 |
| **Synthesis complete** | Themes finalized, need decisions | Steps 4–5 → Step C → Dashboard Tabs 3–4 |
| **Reporting** | Research done, need to communicate | Step 6 → Full dashboard |
| **Scheduling needed** | "Schedule," "invite," "reach out" mentioned | Step E |
| **Problem framing** | "What's the real problem," "problem statement" | Step C |
| **Friction/sentiment ask** | "Where's the friction," "how do people feel" | Step D |

### A2. Output Pipeline Status Card
At the start of every response, output this card (keep it short):

```
📊 Pipeline Status
Stage: [current stage]
Done: [what's been completed]
Next: [1–2 most important next actions]
Blockers: [gaps in coverage, missing data, or unanswered open questions — or "None"]
```

### A3. Maintain Session State
Track and update this state object across the session:
```
{
  stage: "...",
  stakeholders_total: 0,
  interviews_done: 0,
  themes_identified: 0,
  problem_statements: 0,
  dashboard_version: 0,
  coverage_gaps: [],
  open_questions: []
}
```

---

## Step P — Interview Planning (when user hasn't started yet)

Trigger when: user says "I need to plan my research," "who should I interview," "help me prepare," "what questions should I ask," "set up my research plan," or is at the blank-slate stage.

### P1. Define Research Goals
Ask or infer:
- **What decision will this research inform?** (roadmap priority, go/no-go, feature design, strategy)
- **What do you already believe?** (list assumptions to validate)
- **What would change your mind?** (identify the falsifiable hypotheses)

### P2. Stakeholder Mapping
Build a stakeholder interview plan organized into tiers:

| Tier | Who | Why Interview Them | Priority |
|---|---|---|---|
| Tier 1 — Decision makers | Executives, VPs, Directors | Strategic context, budget, blockers | Must-have |
| Tier 2 — Power users | Senior ICs, team leads | Deep workflow knowledge, pain points | Must-have |
| Tier 3 — Adjacent stakeholders | Cross-functional partners | Dependencies, integration points | Important |
| Tier 4 — Skeptics / detractors | Known critics of current state | Stress-test assumptions | Nice-to-have |

Output a **stakeholder interview list** with: Name | Role | Team | Tier | Why them | Status (Scheduled / Pending / Complete)

After generating this list, immediately run **Step B** (Stakeholder Segmentation) on top of it.

### P3. Interview Guide Generation
For each stakeholder tier, generate a tailored interview guide:

**Structure per guide:**
1. **Opener** (2 min) — build rapport, explain purpose, set recording/note-taking expectations
2. **Context questions** (5 min) — understand their world, role, daily workflow
3. **Core questions** (20 min) — probe pain points, JTBD, workarounds, decision factors
4. **Reaction questions** (5 min) — if validating, show concepts/prototypes here
5. **Closer** (3 min) — who else should we talk to? anything we didn't cover?

**Question writing rules:**
- Open-ended only (no yes/no questions in core section)
- "Tell me about a time when..." > "Do you ever..."
- "Walk me through how you currently..." > "Would you use a feature that..."
- Include 2–3 follow-up probes per core question
- Flag which questions are hypothesis-testing vs. discovery

See `templates/interview-guide-tier1.md`, `tier2.md`, `tier3.md` for ready-to-use templates.

### P4. Research Calendar
Suggest a realistic timeline:
- Interviews per week (recommend 3–5 max to allow synthesis time)
- Synthesis sessions (block time after every 3–4 interviews)
- Mid-point check-in (after 50% of interviews — are themes emerging?)
- Final synthesis deadline
- Stakeholder readout date

### P5. Logistics Checklist
- [ ] Consent / recording policy confirmed
- [ ] Note-taking template shared with team
- [ ] Calendar invites sent (trigger **Step E** to generate these)
- [ ] Screener criteria defined (if recruiting externally)
- [ ] Backup interviewees identified

---

## Step B — Stakeholder Segmentation Engine

Run after Step P2 (stakeholder mapping) or any time a stakeholder list is created or updated.

### B1. Behavioral Segmentation
Apply behavioral segments on top of the tier structure:

| Segment | Signals | Interview Approach |
|---|---|---|
| **Champion** | Enthusiastic, actively advocates, shares examples unprompted | Co-creator — involve early, share drafts, ask who else they'd recruit |
| **Pragmatist** | Neutral, wait-and-see, asks "show me proof" | Evidence-first — bring data, case studies, concrete demos |
| **Skeptic** | Resistant, raises risks, defends status quo | Safety-first — acknowledge concerns, probe underlying fears, don't oversell |
| **Uninvolved** | Not yet impacted, low awareness | Awareness-first — help them see relevance before going deep |

Infer segment from: job title + known context + past interactions (if shared). Default to Pragmatist when unknown.

### B2. Influence × Pain Matrix
Rate each stakeholder on two axes:
- **Influence Level**: High (shapes decisions or budgets) / Med (contributes to decisions) / Low (individual impact only)
- **Pain Intensity**: High (actively frustrated, workarounds in place) / Med (aware of friction, tolerating it) / Low (not feeling it yet)

Output a 2×2 matrix:

```
High Influence │ Prioritize & co-create  │  Address concerns first
               │ (High Pain Champions)    │  (High Pain Skeptics)
───────────────┼──────────────────────────┼─────────────────────────
Low Influence  │ Include as validators    │  Nice-to-have coverage
               │ (Low Pain Champions)     │  (Low Pain Skeptics)
               └──────────────────────────┴─────────────────────────
                    High Pain                    Low Pain
```

### B3. Interview Priority Order
Sort the final stakeholder list: High Influence + High Pain first. Generate this as a numbered queue that feeds directly into Step E (scheduling).

---

## Step 0 — Identify What the User Has

Before analyzing, quickly assess the input type(s):

| Input Type | How to Handle |
|---|---|
| Interview transcript / notes | Extract quotes, pain points, jobs-to-be-done, sentiment |
| Survey responses | Aggregate, find patterns, flag outliers |
| Meeting notes / Slack threads | Extract decisions, blockers, implicit asks |
| Recorded session summary | Treat like transcript; note behavioral observations |
| Document / spec / report | Extract requirements, assumptions, gaps |
| Multiple inputs at once | Run synthesis across all; note source per insight |

Ask the user if it's unclear what type of input they're providing.

---

## Step 1 — Deep Analysis of Each Input

For every input, extract and structure the following:

### 1A. Speaker Profile
- **Name / Role / Team** (if known)
- **Seniority level**: Executive / Senior IC / Mid / Junior
- **Interview type**: Discovery / Validation / Generative / Evaluative
- **Date** (if available)

### 1B. Raw Signal Extraction
Pull out verbatim or near-verbatim quotes for:
- **Pain points** — what's frustrating, broken, slow, or missing
- **Jobs to be done** — what outcome they're trying to achieve
- **Workarounds** — how they cope today
- **Bright spots** — what's working well
- **Implicit asks** — things they didn't directly request but implied
- **Decision-making signals** — what would make them adopt / reject a solution

### 1C. Sentiment & Confidence
- Overall sentiment: Positive / Neutral / Negative / Mixed
- Confidence in statements: High / Medium / Low (watch for hedging language)
- Emotional intensity markers: Frustration / Urgency / Excitement / Resignation / Confusion
- Score sentiment −2 (very negative) to +2 (very positive) for Step D aggregation
- Score urgency 1 (low) to 5 (high) for friction heatmap

After extracting sentiment from each input, immediately run **Step D** to update the heatmap.

---

## Step 2 — Critical Evaluation

Do NOT just summarize what people said. Critically evaluate the research quality and signal strength.

### 2A. Signal Quality Check
For each insight ask:
- Is this **one person's opinion** or **corroborated across multiple sources**?
- Is the person **close to the problem** or speaking hypothetically?
- Is this a **stated need** or an **observed behavior**? (Observed > stated)
- Is there **recency bias** — is this top-of-mind vs. a persistent pain?

### 2B. Bias & Gap Detection
Flag any of the following:
- **Selection bias** — are we only hearing from power users or vocal critics?
- **Leading questions** — did the interviewer's framing likely shape the answer?
- **Coverage gaps** — which roles / teams / use cases are NOT represented yet?
- **Contradictions** — where do different interviewees disagree?

### 2C. Confidence Scoring
Rate each theme/insight on a 1–5 confidence scale:
- 5 = Corroborated by 3+ independent sources, observed behavior
- 4 = Mentioned by 2–3 sources with specificity
- 3 = Single strong source or multiple weak sources
- 2 = Speculative, hedged, or second-hand
- 1 = Anecdotal, unverified, or contradicted elsewhere

---

## Step 3 — Synthesis & Theming

Group insights into themes using affinity mapping logic:

1. List all extracted pain points / asks / signals
2. Group by underlying job-to-be-done (not surface feature)
3. Name each theme using a verb phrase: e.g., "Reduce time spent on manual reconciliation" not "Reconciliation issues"
4. For each theme, record:
   - **Theme name** (verb phrase)
   - **Supporting quotes** (2–3 max, with source)
   - **Affected roles/teams**
   - **Frequency** (how many sources mentioned it)
   - **Confidence score** (from Step 2C)
   - **Current workaround** (if any)

After theming is complete, immediately run **Step C** (Problem Statement Generation) for each theme.

---

## Step C — Problem Statement Generation

Run automatically after Step 3 (theming). For each theme, generate all four outputs:

### C1. HMW Statement (How Might We)
Format: "How might we help [persona] [achieve outcome] without [key constraint]?"

Rules:
- One HMW per theme — keep it to one sentence
- Focus on the outcome, not the solution
- The constraint anchors it so it's not too broad

Example: "How might we help Finance Managers close the books at month-end without spending 3+ hours on manual data exports?"

### C2. JTBD Narrative (Job-to-be-Done)
Format: "When I [situation/trigger], I want to [motivation/capability], so I can [expected outcome/progress]."

Rules:
- Written in first-person from the persona's perspective
- The "so I can" clause must name a meaningful outcome, not just a feature
- Avoid solution language in the "I want to" clause

Example: "When I close the books at month-end, I want the data from all systems to reconcile automatically, so I can file accurate reports in hours instead of days."

### C3. 5 Whys Drill-down
Start from the surface symptom (what the user complained about) and drill down 5 levels:

Format:
1. **Surface**: [what the user said]
2. **Why?**: [first-order cause]
3. **Why?**: [second-order cause]
4. **Why?**: [third-order cause]
5. **Why?**: [fourth-order cause]
6. **Root cause**: [underlying system failure or organizational gap]

Stop at the level where the answer is a policy, incentive, organizational structure, or system architecture decision — that's the root cause.

### C4. Problem Brief (one-pager)
Output this structure for each theme:

```
PROBLEM BRIEF: [Theme Name]

Problem: [1–2 sentence description of what's broken]
Affected personas: [role names]
Impact: [quantified if possible — time lost, errors, revenue, etc.]
Evidence: [2–3 supporting quotes with source]
Root cause: [from 5 Whys]
Current workaround: [how people cope today]
Constraints: [what makes this hard to solve]
Success metric: [how we'd know the problem is solved]
Status: Draft | Validated | Accepted | Deferred
```

---

## Step D — Friction & Sentiment Heatmap

Run after Step 1C (sentiment extraction). Updates continuously as new interviews are added.

### D1. Per-Stakeholder Sentiment Record
For each completed interview, record:
- Sentiment score: −2 to +2
- Urgency score: 1–5
- Friction areas mentioned (process areas, not features)
- Emotional markers (Frustration / Urgency / Excitement / Resignation / Confusion)
- Interview date (for timeline)

### D2. Friction Heatmap by Process Area
Aggregate across all stakeholders:

| Process Area | Team A | Team B | Team C | Total |
|---|---|---|---|---|
| [Area 1] | 4 | 2 | 0 | 6 |
| [Area 2] | 1 | 3 | 3 | 7 |

Score per cell = sum of urgency scores from stakeholders on that team who mentioned that area.
Color coding: 0–2 = green, 3–4 = yellow, 5+ = red.

### D3. Cross-Stakeholder Pattern Detection
Flag when 3+ stakeholders mention friction in the same process area — this is a strong signal regardless of confidence score.

Output: "**Convergence alert**: [X] stakeholders across [Y] teams all express friction around [process area]. This is a high-confidence friction point even if themes are still forming."

### D4. Sentiment Timeline
Track sentiment score per stakeholder per interview date. If a stakeholder has been interviewed multiple times, show trend direction (improving / worsening / stable).

---

## Step E — Interview Scheduling Pack

Trigger when: "schedule," "invite," "reach out," "set up interviews," or Step P5 fires.

### E1. Scheduling Priority Queue
Sort stakeholders from Step B3 (influence × pain priority) and output as a numbered queue:
1. [Name] — Tier 1, Champion, High Influence + High Pain → Interview first
2. [Name] — Tier 2, Pragmatist, High Pain → Interview second
...

### E2. Per-Stakeholder Scheduling Pack
For each stakeholder, generate three assets:

**Email Draft:**
```
Subject: Quick chat — [research topic], 30 min?

Hi [Name],

I'm leading a research effort to [research goal in 1 sentence]. Given your role in [their area], your perspective would be really valuable.

Would you be open to a 30-minute conversation this week or next? I'll come prepared with a few focused questions — no prep needed on your end.

[Calendar link placeholder] or feel free to suggest a time that works.

Thanks,
[Your name]
```

**Calendar Invite Body:**
```
[Research Topic] — Discovery Interview (30 min)

Agenda:
• Context (5 min) — Brief overview of the research and your role
• Core questions (20 min) — Your day-to-day workflow and pain points
• Wrap-up (5 min) — Open questions, who else to talk to

Pre-read: [Link placeholder — optional]
Meeting link: [Zoom/Teams placeholder]

No preparation needed. This is a listening session, not a review.
```

**Slack DM Variant:**
```
Hey [Name] — I'm doing some research on [topic] and your perspective would be really helpful. Would you have 30 min sometime this week for a quick chat? Happy to work around your schedule.
```

### E3. Scheduling Status Tracker
Maintain status for each stakeholder: Not Started → Reached Out → Responded → Scheduled → Complete

Auto-suggest next outreach: "5 stakeholders not yet reached out to. Suggest prioritizing: [Name 1] (Tier 1), [Name 2] (Tier 1)."

See `templates/scheduling-pack.md` for the full template set.

---

## Step 4 — Use Case & Opportunity Mapping

For each theme, identify:

### Opportunity Statement
> "When [persona] tries to [job-to-be-done], they struggle with [pain], which causes [impact]. A solution that [capability] would allow them to [outcome]."

### Use Case Variants
- **Primary use case** — the most common scenario
- **Edge cases** — important but less frequent scenarios
- **Anti-use-cases** — explicitly out of scope based on what you heard

### Effort vs. Impact Matrix
Rate each opportunity:
- **User Impact**: High / Medium / Low
- **Frequency**: Daily / Weekly / Monthly / Rare
- **Implementation Complexity**: High / Medium / Low (rough estimate only)
- **Strategic Fit**: Core / Adjacent / Exploratory

---

## Step 5 — Roadmap Building

Build a prioritized roadmap from the opportunities.

### Prioritization Framework

| Criterion | Weight |
|---|---|
| User Impact | 30% |
| Frequency of pain | 25% |
| Confidence in signal | 20% |
| Strategic fit | 15% |
| Ease of implementation | 10% |

### Roadmap Output Format

**Now (0–3 months)** — High confidence, high frequency, lower complexity
**Next (3–6 months)** — High impact, medium confidence, may need more research
**Later (6–12 months)** — Strategic bets, exploratory, lower confidence
**Parking Lot** — Interesting but insufficient signal; needs more research

For each item include:
- Feature / initiative name
- Problem it solves (one line)
- Personas affected
- Confidence score
- What would validate it further

---

## Step 6 — Stakeholder Report Generation

When the user asks for a report or summary for leadership / cross-functional sharing:

### Report Structure
1. **Executive Summary** (3–5 bullets, outcomes-focused)
2. **Research Coverage** (who was interviewed, when, gaps)
3. **Top 3–5 Themes** (with evidence and confidence)
4. **Problem Statements** (HMW + JTBD for top themes)
5. **Opportunity Map** (use cases ranked)
6. **Recommended Roadmap** (Now/Next/Later)
7. **Open Questions** (what we still need to learn)
8. **Appendix** (full quotes by theme)

Tone: Direct, data-backed, decisive. Avoid hedging. Say "the data suggests X" not "some people maybe felt X."

---

## Step 7 — Editable Stakeholder Dashboard (PRIMARY OUTPUT)

**This is the default primary output of the skill.** Build it whenever there is research data to show — don't wait to be asked. After any analysis (Steps 1–6), always offer the dashboard.

The dashboard is a single React artifact. It is:
- **Ordered by stakeholder** — every view can be filtered/grouped by person or role
- **Editable inline** — users can click any field and update it directly in the artifact
- **Simple but complete** — 9 tabs for each major view, no clutter

→ See `references/dashboard-spec.md` for the full implementation spec.

### Dashboard Tabs (in order)

**Tab 1: Stakeholders** ← DEFAULT VIEW
- Table ordered by: Tier → Seniority → Name
- Columns: Name | Role | Team | Tier | Segment | Influence | Pain | Interview Status | Sentiment | Key Theme | Date
- Inline editable: Status, Sentiment, Segment, Notes
- Color-coded status badges (Scheduled / In Progress / Complete / Not Started)
- Click any row to expand full notes + quotes for that person

**Tab 2: Themes**
- Cards grouped by theme, each showing:
  - Theme name (editable)
  - Stakeholders who mentioned it (linked to Tab 1)
  - Confidence score (editable 1–5 slider)
  - Top quote per stakeholder
- Sort by: Confidence, Frequency, Impact

**Tab 3: Opportunities**
- Simple prioritized list (not a complex matrix)
- Columns: Opportunity | Stakeholders | Impact | Effort | Roadmap Bucket
- All fields inline editable
- Drag to reorder priority

**Tab 4: Roadmap**
- Now / Next / Later / Parking Lot swimlanes
- Each card: Initiative | Problem | Stakeholders | Confidence
- Editable: move cards between swimlanes, edit titles

**Tab 5: Interview Planner**
- Stakeholder list with interview status
- Question guide per stakeholder tier (expandable)
- Coverage gap alerts (roles not yet interviewed shown in red)
- Add new stakeholder button

**Tab 6: Open Questions**
- Running list of things still to learn
- Each item: Question | Why it matters | Who to ask | Status
- All editable inline

**Tab 7: Problem Statements** ← NEW
- Card per problem statement, ordered by impact
- Each card: HMW (bold headline) → JTBD narrative → 5 Whys (collapsible) → Affected personas → Status badge
- Status: Draft / Validated / Accepted / Deferred (editable)
- "Copy Problem Brief" button per card
- Filter by: Status, Theme, Persona

**Tab 8: Sentiment & Friction** ← NEW
- Top: Friction Heatmap — grid of Process Area × Team, color-coded (green → yellow → red)
- Bottom: Sentiment Timeline — sparkline per stakeholder (date × score)
- Click any heatmap cell → show quotes from stakeholders in that area
- Filter: by team, by process area, by date range

**Tab 9: Pipeline Status** ← NEW
- Left: Coverage tracker — % of each tier interviewed, red highlights on gaps
- Center: Scheduling board — Kanban: Not Started → Reached Out → Scheduled → Complete
- Right: Next actions — auto-generated checklist ("Interview [Name] — Tier 1, not yet scheduled")
- Pipeline health score (0–100): based on coverage %, confidence levels, open question count

### Data Population Rule
Always populate the dashboard with real data from the analysis — never leave it as a blank template. If no research has been shared yet, populate with a realistic sample for the user's context (B2B enterprise, internal stakeholders) and label it clearly as sample data.

---

## Step 8 — Continuous Improvement Protocol

After each research session:

1. **Note what questions weren't answered** — add to "Open Questions" tracker
2. **Flag new interview patterns** — update the extraction framework if new signal types emerge
3. **Track coverage** — maintain a running list of who has/hasn't been interviewed
4. **Calibrate confidence scores** — if a "low confidence" finding later got validated, note it

### Self-Improvement Triggers
When asked "How is my research practice improving?", provide:
- Coverage analysis (who's missing from the research)
- Question quality review (are interview questions generating rich signal?)
- Bias audit (are we interviewing diverse enough perspectives?)
- Confidence trend (are findings getting stronger or staying weak?)

---

## Output Defaults

| User says / does | Default output |
|---|---|
| "Start my research" / blank slate | Step A → Step P → Step B → Step E (scheduling pack) |
| "Who should I interview?" | Step P2 → Step B (segmentation + 2×2 matrix) |
| "Write interview questions" | Step P3 → link to tier templates |
| "Schedule my interviews" | Step E (full scheduling pack for all pending) |
| Drops in a transcript | Step A → Steps 0–3 → Step D → Dashboard update |
| "What themes are emerging?" | Step 3 → Dashboard Tab 2 |
| "What are the real problems?" | Step C → Dashboard Tab 7 |
| "Show me where the friction is" | Step D → Dashboard Tab 8 |
| "What's left to do?" | Step A → Dashboard Tab 9 |
| "Build a roadmap" | Steps 4–5 → Dashboard Tab 4 |
| "Write a stakeholder report" | Step 6 (full report with problem statements) |
| "Show me a dashboard" | Step 7 (full 9-tab dashboard artifact) |
| "Review my research" | Step 8 (improvement audit) |
| Pastes raw notes with no instruction | Steps A, 0–3, D → auto-build/update Dashboard |

---

## Tone & Style Guidelines

- Be a **critical thinking partner**, not just a summarizer
- Flag weak signal explicitly — don't let low-confidence findings inflate the roadmap
- Use **PM language**: jobs-to-be-done, opportunity sizing, signal strength, coverage, HMW
- When quoting interviewees, preserve their voice — don't sanitize or over-paraphrase
- Always distinguish **what was said** from **what it means**
- End every analysis with: **"What would strengthen this further"** — 2–3 specific next research actions

---

## Quick Reference — Signal Strength Vocabulary

| Phrase to use | Meaning |
|---|---|
| "Strong signal" | 3+ corroborated sources, observed behavior |
| "Emerging pattern" | 2 sources, needs validation |
| "Single-source insight" | Interesting but unconfirmed |
| "Hypothesis" | Derived from inference, not direct statement |
| "Noise" | Contradicted or likely outlier |
| "Convergence alert" | 3+ stakeholders express friction in same area |
