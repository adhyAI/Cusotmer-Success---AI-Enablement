# pm-skills — Claude Instructions

This repository is a PM skills toolkit. When someone opens this repo, they are a Product Manager (or aspiring PM) running structured research or product work with AI assistance.

---

## On First Open

1. **Check for `user-research/input.md`** — if it exists and has been filled in (fields are not all empty strings), read it in full before doing anything else. This file contains the PM's name, research topic, stakeholders, assumptions, and context. Use it to personalize every output.

2. **If `input.md` is blank or missing**, greet the user and prompt them to fill it in:
   > "Welcome to pm-skills. Before we start, fill in `user-research/input.md` — it takes about 5 minutes and tells me who you are, what you're researching, and who you're interviewing. Once it's filled in, come back and say 'start' and I'll take it from there."

3. **If `input.md` is filled in**, immediately run Step A (Pipeline Orchestration) from the skill and output:
   - A Pipeline Status Card (stage, what's done, what's next, blockers)
   - A segmented stakeholder list (from Step B)
   - The interview priority queue
   - Offer to generate the scheduling pack

---

## Active Skill

The active skill for this repository is **User Research PM Pipeline**.

- Skill definition: `user-research/SKILL.md`
- User context: `user-research/input.md`
- Dashboard spec: `user-research/references/dashboard-spec.md`
- Pipeline logic: `user-research/references/pipeline-spec.md`
- Templates: `user-research/templates/`

**Always read `user-research/SKILL.md` in full before responding to any research-related request.** It contains the step definitions, routing logic, output formats, and tone guidelines.

---

## Data Architecture

| Location | What goes here | Committed to git? |
|---|---|---|
| `user-research/input.md` | Research project context, stakeholder list, goals | Yes (it's setup config) |
| `data/` | Raw interview transcripts, survey exports, notes | **No — gitignored** |
| `outputs/` | Generated reports, dashboard exports, scheduling packs | **No — gitignored** |
| `user-research/templates/` | Reusable templates (interview guides, trackers) | Yes — these are shared resources |
| `user-research/references/` | Skill specs and dashboard implementation | Yes — these are system files |

**Never write personal data (names, quotes, interview content) to files outside `data/` or `outputs/`.** Those folders are gitignored for privacy.

---

## Key Behaviors

- **Auto-route inputs**: When the user pastes a transcript, notes, or stakeholder list, detect the input type and route to the correct step without asking. Only ask for clarification if it's genuinely ambiguous.

- **Always output a Pipeline Status Card first**: Every response starts with the card showing current stage, what's done, what's next, and any blockers.

- **Personalize from input.md**: Use the PM's name, their research topic, their stakeholder names, and their stated assumptions throughout. Never use generic placeholder names when real ones are available.

- **Dashboard is the default output**: After any analysis step, offer to build or update the 9-tab React dashboard. Don't wait to be asked.

- **Be a critical partner**: Flag weak signal, call out coverage gaps, and push back on low-confidence findings making it into the roadmap. The goal is better decisions, not validated assumptions.

- **Never auto-submit or auto-send anything**: Scheduling emails and calendar invites are generated as drafts for the PM to review and send manually.

---

## Trigger Phrases → Skill Steps

| User says | Route to |
|---|---|
| "start" / "start my research" / "read my input.md" | Step A → Step B → Step E |
| Pastes a transcript or interview notes | Steps 0–3 → Step D → Dashboard update |
| "what are the real problems" / "problem statement" | Step C |
| "where's the friction" / "sentiment" | Step D → Dashboard Tab 8 |
| "schedule my interviews" / "reach out" | Step E |
| "what's left" / "pipeline status" | Step A → Dashboard Tab 9 |
| "build a roadmap" | Steps 4–5 → Dashboard Tab 4 |
| "write a report" / "stakeholder report" | Step 6 |
| "show me the dashboard" | Step 7 (full 9-tab React artifact) |

---

## Active Skills

| Folder | Skill | Trigger |
|---|---|---|
| `user-research/` | User Research PM Pipeline | Interview transcripts, stakeholder analysis, research planning |
| `meeting-notes/` | Meeting Notes Highlights Builder | Any meeting notes, call transcripts, session summaries |
| `prioritization/` | Opportunity Prioritization | Scoring, RICE, roadmap building, "what should we build first" |
| `project-management/` | Project Management | Status tracking, milestones, blockers, retros, exec updates |
| `outcomes-measurement/` | Outcomes Measurement | Success metrics, baselines, KPIs, "did it work," impact reviews |
| `skill-builder/` | Skill Builder (meta) | "Build me a skill," "create an agent for," new workflow automation |

**Read the relevant `SKILL.md` before responding to any request that matches a skill's trigger conditions.**

### Auto-build skills
When the user describes a repeatable workflow that doesn't match an existing skill, activate `skill-builder/SKILL.md` to design and write a new `SKILL.md`. Place it in a new folder at the repo root alongside the existing skills.

## Future Skills (Coming)

- `stakeholder-comms/` — Status updates, exec briefings, communication templates
- `go-to-market/` — GTM planning, launch checklists, messaging frameworks
- `product-strategy/` — Market framing, opportunity sizing, strategic narrative
