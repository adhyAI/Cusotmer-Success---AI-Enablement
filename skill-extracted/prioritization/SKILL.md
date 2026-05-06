---
name: prioritization
description: >
  Opportunity prioritization and scoring skill. Trigger when the user wants to prioritize a backlog, score opportunities, rank features, decide between competing options, build a roadmap, or evaluate effort vs. impact. Also trigger on: "what should we build first," "score these opportunities," "prioritize this list," "effort vs. impact," "RICE score," "what's the highest leverage move," "rank these ideas," "help me prioritize," "what are the quick wins," "which of these should we do," "build a roadmap from this," "MoSCoW," or "opportunity scoring." Activate for any decision about what to work on, in what order, with what resources. Produces scored opportunity cards, a prioritized roadmap, and a Prioritization Matrix tab update.
---

# Prioritization Skill

## Purpose
Turn a list of ideas, problems, opportunities, or backlog items into a scored, ranked, roadmap-ready output. The primary framework is RICE (Reach × Impact × Confidence ÷ Effort), augmented with horizon assignment (Now/Next/Later), anti-use-case flagging, and success metric definition. Output feeds the Prioritization Matrix dashboard tab directly.

---

## Step A — Intake & Framing

### A1. Capture Inputs
Accept any of:
- A list of feature ideas or backlog items
- Themes or problem statements from user research
- "What should we do about X?" style open questions
- Existing opportunity descriptions
- A filled prioritization matrix to re-score

### A2. Establish the Scoring Context
Before scoring, confirm or infer:
- **Who is affected?** (defines Reach denominator — is it 10 TCs or 500 enterprise customers?)
- **What counts as "impact"?** (time saved, revenue protected, CSAT improvement, risk reduction)
- **What counts as "effort"?** (engineering weeks, full product build, configuration only)
- **What's the time horizon?** (quarterly, H1/H2, annual)
- **Are there hard constraints?** (platform dependencies, budget limits, team size)

If scoring context is ambiguous, default to relative scoring (each opportunity scored relative to others, not on absolute scale).

---

## Step 1 — Opportunity Definition

### 1A. Structure Each Opportunity
For each item, extract or define:
```
Title: [Short, action-oriented name]
Problem it solves: [1 sentence]
Who benefits: [persona or stakeholder group]
When they need it: [situation/trigger]
What they can do differently: [the outcome]
Evidence: [supporting quotes, data, or interview signals]
```

### 1B. Anti-Use-Case Check
For each opportunity, identify at least one scenario where this solution is the WRONG answer:
- Specific user type it would confuse or harm
- Edge case where it breaks down
- Scenario where the problem is better solved another way

Anti-use-cases prevent over-indexing on a solution before its boundaries are understood.

### 1C. Success Metric Definition
Define 1–3 measurable success metrics per opportunity:
- **Format**: [Metric name]: [Current state] → [Target state]
- **Example**: "Version upgrade time: 20 min manual → <2 min automated"
- Prefer leading indicators (behavior change) over lagging indicators (outcome achieved)
- Flag any metric that requires a baseline we don't yet have: `[BASELINE NEEDED]`

---

## Step 2 — RICE Scoring

### 2A. Scoring Dimensions

Score each dimension 1–10 relative to the other opportunities:

| Dimension | 1–10 | Guidance |
|---|---|---|
| **Reach** | Low=few users / High=most users | How many people experience this problem regularly? |
| **Impact** | Low=marginal / High=transformative | How much does this change their experience per occurrence? |
| **Confidence** | Low=speculative / High=validated | How confident are we in these estimates? Are they grounded in data? |
| **Effort** | Low=trivial / High=enormous | How hard to build? (Higher effort = harder) |

RICE Score = (Reach × Impact × Confidence) ÷ Effort

### 2B. Confidence Calibration
Confidence is the most important and most commonly inflated score. Apply these adjustments:

| Signal | Confidence modifier |
|---|---|
| Validated in 3+ interviews | +2 |
| Single data point | −2 |
| Based on assumption (not observed) | −3 |
| Technically unclear how to build | −2 |
| Directly requested by high-influence stakeholder | +1 |
| Existing solution attempted and failed | −2 |

### 2C. Score Table Output
```markdown
## RICE Scores — [Date]

| Rank | Opportunity | Reach | Impact | Conf | Effort | RICE | Horizon |
|---|---|---|---|---|---|---|---|
| 1 | [Title] | R | I | C | E | score | Now/Next/Later |
...

**Scoring context:** [1–2 sentences on what the scores are relative to]
**Confidence note:** [flags on any low-confidence scores that drive the ranking]
```

---

## Step 3 — Effort × Impact 2×2

### 3A. Classify Each Opportunity
Assign each opportunity to a quadrant based on relative scoring:

| Quadrant | Effort | Impact | Strategy |
|---|---|---|---|
| **Quick Wins** | Low–Med | High | Do now — these build momentum |
| **Strategic Bets** | High | High | Invest carefully — validate first |
| **Fill-Ins** | Low | Low | Do only if no quick wins remain |
| **Rethink** | High | Low | Don't do — or find a leaner version |

### 3B. 2×2 Table Output
```markdown
## Effort × Impact Matrix

**Quick Wins** (do now)
- [opportunity], RICE: [score]

**Strategic Bets** (invest with validation)
- [opportunity], RICE: [score]

**Fill-Ins** (low priority)
- [opportunity], RICE: [score]

**Rethink / Don't Do**
- [opportunity] — [reason why it's low leverage]
```

---

## Step 4 — Horizon Assignment

### 4A. Assign Now / Next / Later
Rules:
- **Now**: High RICE + low effort + validated pain + no dependencies
- **Next**: High RICE but requires medium effort OR needs a prerequisite from Now
- **Later**: Lower RICE, complex dependencies, or confidence too low to commit

**Parking Lot**: Good idea but missing validation, blocked, or out of scope. Don't assign to Now/Next/Later yet.

### 4B. Dependency Check
Before finalizing horizons, check:
- Does any "Next" item depend on a "Now" item to be built first?
- Does any "Later" item become higher priority if a certain assumption is validated?
- Are there any quick wins that would be blocked by a missing data model or integration?

### 4C. Roadmap Output
```markdown
## Roadmap — Now / Next / Later

### Now
| Opportunity | RICE | Effort | Owner | Success Metric |
|---|---|---|---|---|

### Next
| Opportunity | RICE | Effort | Dependencies | Success Metric |
|---|---|---|---|---|

### Later
| Opportunity | Horizon Trigger | What Would Move It Forward |
|---|---|---|

### Parking Lot
| Opportunity | Blocked By | Revisit When |
|---|---|---|
```

---

## Step 5 — Trade-off Analysis

### 5A. Surface the Hard Choices
Identify any cases where:
- Two high-RICE items compete for the same resource or team
- A quick win might undermine a more important strategic bet
- Stakeholder pressure (not signal) is inflating a specific item's priority

For each trade-off, write: "Choosing [A] over [B] means accepting [consequence] because [rationale]."

### 5B. Assumptions to Validate
List the assumptions embedded in the top 3 ranked items. For each:
- What would change the RICE score if this assumption is wrong?
- How can it be validated cheaply? (1 interview, a quick survey, a spike, a prototype)

---

## Step 6 — Dashboard Update

### 6A. Opportunity Record Format
Output records for `INIT_OPPORTUNITIES` in the dashboard:
```javascript
{
  id: "op[N]",
  title: "[Short name]",
  description: "[1–2 sentence description]",
  themeIds: ["t[N]"],
  stakeholderIds: ["s[N]"],
  reach: [1-10],
  impact: [1-10],
  confidence: [1-10],
  effort: [1-10],
  effortLabel: "S" | "M" | "L" | "XL",
  horizon: "Now" | "Next" | "Later",
  status: "Candidate" | "Under Evaluation" | "Approved" | "Rejected" | "In Progress" | "Delivered",
  tags: ["[tag]"],
  successMetrics: ["[metric: baseline → target]"],
  antiUseCases: ["[scenario where this is wrong]"],
  notes: "[context for this opportunity]"
}
```

Effort label guide: S = effort 1–3 | M = 4–6 | L = 7–8 | XL = 9–10

---

## Quality Standards

| Check | Requirement |
|---|---|
| **Confidence discipline** | No opportunity scores Confidence >7 without interview validation |
| **Anti-use-cases** | Every approved opportunity has at least one anti-use-case |
| **Success metrics** | Every Now/Next item has at least one measurable metric |
| **Trade-offs explicit** | If two items compete, the trade-off is named, not hidden |
| **Assumptions surfaced** | Top 3 items have their key assumptions listed |
| **RICE formula visible** | Score table always shows all 4 components, not just the final score |

---

## Frameworks Reference

**MoSCoW** (use when there's a fixed release scope):
- Must Have / Should Have / Could Have / Won't Have

**Opportunity Solution Tree** (use when exploring solution space):
- Desired Outcome → Opportunities → Solutions → Experiments

**Weighted Scoring** (use when RICE doesn't fit the domain):
- Define 4–6 custom criteria → weight them (total = 100%) → score each option 1–5

**ICE** (use for speed over precision):
- Impact × Confidence × Ease (no Reach) — good for early-stage exploration

Default: **RICE** unless the user specifies otherwise or the domain has unusual constraints.
