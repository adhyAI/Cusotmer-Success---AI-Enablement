# pm-skills

An open-source AI skills toolkit for Product Managers — built for Claude, designed to turn raw research inputs into decisions.

Each skill is a structured intelligence layer that activates automatically. Drop in your data and the skill routes it, analyzes it, and produces the output. No prompting required beyond the setup.

---

## Skill 1: User Research Pipeline

Covers the full research lifecycle — from a blank stakeholder list to a final report with roadmap. The pipeline is automated: once you fill in your project context, Claude handles routing, analysis, synthesis, and dashboard generation.

**What it does:**

| Capability | Output |
|---|---|
| Stakeholder segmentation | Champion / Pragmatist / Skeptic / Uninvolved + Influence × Pain 2×2 matrix |
| Interview guides | Tailored question sets for Tier 1 (execs), Tier 2 (power users), Tier 3 (partners) |
| Scheduling pack | Email draft + calendar invite + Slack DM per stakeholder, prioritized queue |
| Transcript analysis | Pain points, JTBD, workarounds, sentiment, friction areas — extracted and structured |
| Problem statement generation | HMW + JTBD narrative + 5 Whys root cause + full problem brief per theme |
| Friction & sentiment heatmap | Process area × team grid + sentiment timeline across all interviewees |
| Opportunity mapping | Effort/impact scoring, use case variants, anti-use-cases |
| Roadmap | Now / Next / Later / Parking Lot with confidence scores |
| Stakeholder report | Executive summary, themes, problem statements, roadmap, open questions |
| 9-tab editable dashboard | React artifact with all research data, inline editable, stakeholder-ordered |

---

## Quickstart

### 1. Clone the repo

```bash
git clone https://github.com/[your-username]/pm-skills.git
cd pm-skills
```

### 2. Fill in your project context

Open `user-research/input.md` and fill in:
- Your name and role
- What you're researching and what decision it informs
- Your stakeholders (names, roles, tiers)
- Your assumptions and hypotheses
- Timeline and output preferences

This takes about 5 minutes. The more context you provide, the less you'll need to explain to Claude in every session.

### 3. Open Claude

**Option A — Claude Code (recommended):**
Open this folder in Claude Code (CLI or VS Code extension). `CLAUDE.md` loads automatically.

```bash
claude  # from the pm-skills directory
```

Then say: `read my input.md and start the research pipeline`

**Option B — Claude.ai (web/app):**
1. Open `CLAUDE.md` and copy its full contents
2. Start a new conversation and paste it as your first message
3. Then say: `read my input.md` and paste the contents of `user-research/input.md`

### 4. Run

Say `start` — Claude will output:
- A Pipeline Status Card (current stage, what's next, blockers)
- Your segmented stakeholder list with interview priority queue
- Scheduling pack: email drafts, calendar invites, Slack DMs for each stakeholder
- Interview guides tailored to each tier

As you complete interviews, paste transcripts or notes and the pipeline updates automatically.

---

## How to Add Interview Data

Paste transcripts, notes, or survey exports directly into the chat. Label them so Claude routes correctly:

```
This is an interview transcript with [Name, Role] on [Date]:
[paste transcript]
```

```
These are survey responses from [Team] about [Topic]:
[paste responses]
```

```
These are meeting notes from [Meeting name] on [Date]:
[paste notes]
```

Claude will extract signals, update the stakeholder dashboard, run sentiment scoring, and — once 3+ interviews are complete — generate problem statements and update the friction heatmap automatically.

---

## Folder Structure

```
pm-skills/
├── CLAUDE.md                          ← Claude reads this automatically on open
├── README.md                          ← This file
├── .gitignore                         ← Keeps your research data private
│
├── user-research/
│   ├── input.md                       ← FILL THIS IN FIRST
│   ├── SKILL.md                       ← Full skill definition (routing, steps, outputs)
│   ├── references/
│   │   ├── dashboard-spec.md          ← 9-tab React dashboard implementation spec
│   │   └── pipeline-spec.md          ← Pipeline automation, state management, integrations
│   └── templates/
│       ├── interview-guide-tier1.md  ← Decision Makers (45–60 min, 10 questions)
│       ├── interview-guide-tier2.md  ← Power Users (45–60 min, 10 questions)
│       ├── interview-guide-tier3.md  ← Cross-functional Partners (30–40 min, 6 questions)
│       ├── stakeholder-tracker.md    ← Full tracking table with coverage summary
│       └── scheduling-pack.md        ← Email, calendar, and Slack templates per tier
│
├── data/                              ← YOUR RESEARCH DATA GOES HERE (gitignored)
│   └── .gitkeep
└── outputs/                           ← GENERATED REPORTS GO HERE (gitignored)
    └── .gitkeep
```

**Important:** `data/` and `outputs/` are gitignored. Your transcripts, notes, and reports never leave your machine.

---

## Using the Templates Without Claude

All files in `user-research/templates/` are standalone markdown documents. You can use them without Claude:

- Copy any template into Notion, Google Docs, or Obsidian
- Fill in the placeholders manually
- Use the `stakeholder-tracker.md` as a live tracking doc throughout your research

---

## Pipeline Stages

The skill detects your current stage and routes automatically:

```
Blank Slate → Planning → Data Collection → Synthesis → Reporting → Complete
```

At any point, you can ask:
- `"what's left to do?"` → Pipeline status + next actions
- `"show me the dashboard"` → Full 9-tab React dashboard with all current data
- `"what are the real problems?"` → Problem statements (HMW + JTBD + 5 Whys)
- `"where's the friction?"` → Friction heatmap + sentiment timeline
- `"build a roadmap"` → Now/Next/Later roadmap from current themes
- `"write a stakeholder report"` → Full report ready to share

---

## Contributing

New skills, template improvements, and tool integrations are all welcome.

**To propose a new skill** (roadmapping, stakeholder comms, GTM, product strategy):
1. Open an issue with the skill name, trigger conditions, and key outputs
2. Use `user-research/SKILL.md` as the structure template
3. Include a `references/` doc and at least one template

**To improve an existing skill:**
1. Fork → edit → PR with a clear description of what changed and why

---

## License

MIT — use freely, modify freely, share back what you improve.
