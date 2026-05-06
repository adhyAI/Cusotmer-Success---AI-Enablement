---
name: outcomes-measurement
description: >
  Outcomes measurement and success metrics skill. Trigger when the user wants to define success metrics, measure outcomes, track KPIs, evaluate whether something worked, build a measurement plan, assess impact, calculate ROI, run a success review, validate a hypothesis, or answer "did it work?" Also trigger on: "how do we know if this worked," "define success metrics," "build a measurement plan," "track the outcome," "what's the baseline," "set a target," "is this having an impact," "ROI," "KPI," "OKR," "north star metric," "impact assessment," "outcome review," "did this move the needle," or "measure success." Activate for any initiative, feature, or change where you need to know whether the effort produced the intended result.
---

# Outcomes Measurement Skill

## Purpose
Build a rigorous measurement plan for any initiative — from research projects and product features to process improvements and automation rollouts. Covers metric selection, baseline definition, target setting, measurement infrastructure, and outcome review. Designed to ensure that work produces evidence of impact, not just activity.

---

## Step A — Measurement Context

### A1. Classify the Initiative
| Type | Approach |
|---|---|
| **Process improvement** | Before/after comparison on efficiency metric |
| **Product feature** | Usage funnel + outcome metric + guardrail metric |
| **Research project** | Coverage metric + signal quality + decision taken |
| **Automation rollout** | Time/volume/error rate before vs. after |
| **Organizational change** | Behavior change indicator + lagging outcome |
| **Strategic initiative** | Leading indicators during + outcome metric at end |

### A2. Measurement Readiness Check
Before defining metrics, assess:
- **Baseline available?** Can we measure the current state before the change?
- **Attribution possible?** Can we isolate the effect of this initiative vs. other changes?
- **Measurement infrastructure?** Where does the data live? Who can pull it?
- **Timeline sufficient?** Is there enough time to see the effect?

Flag: `[BASELINE NEEDED]` or `[ATTRIBUTION RISK]` where relevant.

---

## Step 1 — Metric Selection

### 1A. Metric Hierarchy

Every initiative needs exactly:
- **1 North Star metric**: The single number that answers "did it work?"
- **2–4 Supporting metrics**: Leading indicators that move before the North Star
- **1–2 Guardrail metrics**: Things that must NOT get worse (quality, satisfaction, cost)

**Do not define more than 7 metrics total.** More metrics = less accountability.

### 1B. Metric Quality Checklist
Each metric must pass all four filters:

| Filter | Question | Failure mode |
|---|---|---|
| **Measurable** | Can we actually get this number? | "Customer satisfaction" without a survey instrument |
| **Attributable** | Does this move when the initiative works? | Metric that's driven by 10 other things |
| **Sensitive** | Does it move fast enough to be useful? | Lagging metric that won't budge for 6 months |
| **Actionable** | If it moves, do we know what to do? | Vanity metric that tells us nothing about cause |

### 1C. Metric Types

| Type | Definition | Example |
|---|---|---|
| **Efficiency** | Time/steps/cost to complete a task | "Version upgrade time: 20 min → 2 min" |
| **Volume** | Count of things happening | "Proposals auto-generated per month" |
| **Quality** | Error rate, accuracy, satisfaction | "Ticket misclassification rate" |
| **Adoption** | % of target users using the thing | "% of TCs using auto-fill after 30 days" |
| **Outcome** | Business result downstream | "Renewal proposal close rate" |
| **Leading indicator** | Early signal of future outcome | "# of health check portal logins/week" |

---

## Step 2 — Baseline Definition

### 2A. Current State Measurement
For each metric, capture the baseline:

```markdown
## Baseline — [Metric Name]

**Current value:** [number or description]
**Measurement date:** [YYYY-MM-DD]
**Source:** [where did this number come from?]
**Confidence:** High (measured) / Med (estimated) / Low (assumed)
**Notes:** [any context that affects interpretation — seasonality, known anomalies]
```

### 2B. Baseline Estimation (when direct measurement isn't possible)
Use structured estimation:
1. **Anchor on a comparable**: "Similar process at [company/team] took X"
2. **Work from components**: Break the activity into steps, estimate each
3. **Ask a practitioner**: The person who does the work estimates their time
4. **Use a range, not a point**: "15–25 min" is more honest than "20 min"

Label estimated baselines: `[ESTIMATED: N–M range]`

### 2C. Baseline Data Gaps
List any metrics where you don't yet have a baseline:
- **What's missing**: [metric]
- **How to get it**: [source, method, owner]
- **By when**: [date you need it before measuring impact]

---

## Step 3 — Target Setting

### 3A. Target Methodology

| Approach | When to use |
|---|---|
| **Evidence-based** | You have data from a comparable implementation |
| **Improvement-based** | X% reduction/increase from current baseline |
| **Benchmark-based** | Industry standard or internal best practice |
| **Hypothesis-based** | Your best estimate of what the solution could achieve |

Always state the basis: "Target is 80% reduction based on [evidence/assumption]."

### 3B. Aspirational vs. Committed Targets
- **Committed target**: What you're confident in achieving. Used for stakeholder reporting.
- **Aspirational target**: What success looks like if everything goes well. Used for product thinking.

Both can coexist. Never present an aspirational target as a commitment.

### 3C. Target Table
```markdown
## Targets — [Initiative Name]

| Metric | Baseline | Committed Target | Aspirational Target | Measurement Date |
|---|---|---|---|---|
| [North Star] | [value] | [value] | [value] | [YYYY-MM-DD] |
| [Supporting 1] | [value] | [value] | [value] | [YYYY-MM-DD] |
| [Guardrail 1] | [value] | Must not exceed [value] | — | [YYYY-MM-DD] |
```

---

## Step 4 — Measurement Plan

### 4A. Measurement Infrastructure
For each metric, define:
| Field | Answer |
|---|---|
| **Data source** | Where does this data live? (Salesforce, FreshDesk, ProTrack, spreadsheet, survey) |
| **Collection method** | Automated pull / manual query / survey / observation |
| **Frequency** | Daily / weekly / monthly / at milestone |
| **Owner** | Who is responsible for pulling and reporting this number? |
| **Report destination** | Dashboard tab / Slack channel / email / meeting |

### 4B. Measurement Timeline
| Checkpoint | Date | What We're Checking |
|---|---|---|
| **Pre-launch baseline** | [T-2 weeks] | Confirm all baselines are captured |
| **Early signal** | [T+2 weeks] | Leading indicators moving? |
| **Mid-point check** | [T+6 weeks] | Supporting metrics trending right? |
| **Outcome read** | [T+12 weeks] | North Star metric evaluated |
| **Retrospective** | [T+14 weeks] | Full impact assessment |

### 4C. Measurement Risks
| Risk | Likelihood | Mitigation |
|---|---|---|
| Baseline wasn't captured before launch | Med | Confirm baseline before shipping |
| Attribution confounded by other initiatives | High | Align with teams shipping in the same window |
| Low adoption → no signal | Med | Adoption rate is a guardrail metric; define minimum N |
| Metric doesn't move in time horizon | Med | Pre-define "leading indicators" that move faster |

---

## Step 5 — Outcome Review

### 5A. Review Format (at each checkpoint)

```markdown
## Outcome Review — [Initiative Name] · [Date]

**Phase:** [Early signal / Mid-point / Final]
**Overall verdict:** Ahead / On track / At risk / Off track / Inconclusive

### North Star
- Target: [value by date]
- Actual: [value]
- Delta: [+N% / −N% / no movement]
- Interpretation: [what does this mean?]

### Supporting Metrics
| Metric | Target | Actual | Status | Interpretation |
|---|---|---|---|---|

### Guardrail Metrics
| Metric | Max allowed | Actual | Status |
|---|---|---|---|

### Verdict
[3–5 sentence assessment: did the initiative work, why, and what to do next]
```

### 5B. Causal Analysis
When a metric doesn't move, avoid jumping to "it didn't work." Investigate:
1. **Adoption check**: Did the target users actually use the thing?
2. **Implementation fidelity**: Was it built and deployed as designed?
3. **Measurement lag**: Is it too early to see the effect?
4. **External interference**: Did something else change that masks the effect?
5. **Wrong metric**: Are we measuring the right thing?

### 5C. Decision from Outcome Review
Every outcome review ends with one of:
- **Continue**: initiative is working, maintain course
- **Amplify**: initiative is working, invest more
- **Adjust**: initiative is partially working, change something specific
- **Pivot**: initiative is not working, fundamentally rethink the approach
- **Stop**: initiative is not working and the problem no longer justifies investment

Document the decision with rationale. This feeds the Results tab.

---

## Step 6 — Impact Communication

### 6A. Impact Summary (for stakeholders)
```markdown
## Impact Summary — [Initiative Name]

**What we shipped:** [1 sentence]
**Why it mattered:** [the problem it solved]

**Results:**
- [North Star metric]: [baseline] → [actual] ([delta %])
- [Supporting metric 1]: [baseline] → [actual]

**What it means:** [business implication — time saved, revenue protected, capacity freed]
**What's next:** [follow-on action or next phase]
```

### 6B. ROI Framing (when applicable)
- **Time saved**: [N hours/week × team size] = [annual hours] × [loaded hourly cost] = [$X/year]
- **Revenue impact**: [metric change] × [conversion or retention assumption] = [$Y]
- **Risk reduction**: [probability of incident] × [cost of incident] × [reduction in probability] = [$Z expected value]

State all assumptions explicitly. Flag estimates vs. actuals.

---

## Quality Standards

| Check | Requirement |
|---|---|
| **Metric count** | Max 7 metrics total (1 North Star + 2–4 supporting + 1–2 guardrails) |
| **Baseline before launch** | Baseline captured before the initiative ships, not after |
| **Targets stated with basis** | Every target states whether it's evidence-based, improvement-based, or hypothesis-based |
| **Attribution acknowledged** | Attribution risks named and mitigated or accepted |
| **Review scheduled** | Outcome review dates on the calendar before launch |
| **Decision from review** | Every outcome review ends with Continue / Amplify / Adjust / Pivot / Stop |

---

## Dashboard Integration

This skill feeds the **Results tab → Outcomes view**:

```javascript
// res[N] — outcome record for INIT_RESULTS
{
  id: "res[N]",
  type: "outcome",
  title: "[Metric name]",
  rationale: "[Why this metric, what it measures]",
  date: "[YYYY-MM-DD]",
  owner: "[Person responsible for measurement]",
  status: "Open" | "In Progress" | "Done",
  horizon: null,
  baseline: "[current state description]",
  target: "[committed target]",
  actual: "[measured value — empty until measured]"
}
```
