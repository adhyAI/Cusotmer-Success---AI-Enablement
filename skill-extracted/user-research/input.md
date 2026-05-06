# User Research — Project Input
> Fill this in before your first session. Claude reads this file automatically to personalize every output — interview guides, stakeholder segmentation, scheduling emails, problem statements, and the dashboard. The more context you provide here, the less you'll need to explain later.

---

## 1. About You

```yaml
your_name: ""                     # Your full name
your_role: ""                     # e.g. "Senior Product Manager"
your_company: ""                  # e.g. "Acme Corp" or leave blank
your_team: ""                     # e.g. "Platform" or "Growth"
research_experience: ""           # Beginner | Some experience | Experienced
```

---

## 2. Research Project

```yaml
project_name: ""                  # Short name for this research effort
research_topic: ""                # One sentence: what area are you investigating?
                                  # e.g. "Why the ops team's monthly reporting process breaks down"

decision_to_inform: ""            # What will this research help you decide?
                                  # e.g. "Whether to build an automated reconciliation tool"

research_type: ""                 # Discovery | Validation | Generative | Evaluative
                                  # Discovery = exploring a problem space you don't fully understand
                                  # Validation = testing a specific hypothesis or concept
                                  # Generative = generating ideas and solutions
                                  # Evaluative = assessing an existing product or design

start_date: ""                    # e.g. "2026-05-10"
target_readout_date: ""           # When do stakeholders expect findings? e.g. "2026-06-15"
```

---

## 3. What You Already Believe

```yaml
assumptions:                      # List the assumptions you're going in with
  - ""                            # e.g. "The problem is worse for finance than engineering"
  - ""
  - ""

hypotheses:                       # What would you bet on being true? (state so we can test them)
  - ""                            # e.g. "Manual data exports are the biggest time sink"
  - ""

what_would_change_your_mind: ""   # What finding would make you completely rethink your approach?
```

---

## 4. Stakeholders

```yaml
# Add everyone you plan to interview or consider interviewing.
# Tier: 1=Decision Maker, 2=Power User, 3=Cross-functional, 4=Skeptic
# Segment: Leave blank — Claude will infer from role and context

stakeholders:
  - name: ""
    role: ""
    team: ""
    tier: 1
    why: ""                       # Why is this person important to interview?

  - name: ""
    role: ""
    team: ""
    tier: 2
    why: ""

  - name: ""
    role: ""
    team: ""
    tier: 2
    why: ""

  - name: ""
    role: ""
    team: ""
    tier: 3
    why: ""

  # Add more as needed
```

---

## 5. Research Scope

```yaml
in_scope:                         # What topics, teams, or processes are you investigating?
  - ""
  - ""

out_of_scope:                     # What are you explicitly NOT investigating?
  - ""

constraints:
  timeline_pressure: ""           # e.g. "Roadmap planning starts June 1"
  known_sensitivities: ""         # e.g. "Avoid questions about the recent reorg"
  budget_or_access_limits: ""     # e.g. "Can only interview internal stakeholders"
```

---

## 6. Output Preferences

```yaml
primary_audience: ""              # Who gets the final report? e.g. "VP Product + Eng lead"
report_tone: ""                   # Executive | Detailed | Technical
dashboard_needed: true            # true | false
scheduling_pack_needed: true      # true | false — generate email/Slack/calendar templates?
preferred_framework: ""           # Leave blank, or specify: "JTBD" | "Jobs + HMW" | "Opportunity solution tree"
```

---

## 7. Additional Context

```yaml
background: |
  # Optional: paste any relevant context here — previous research findings,
  # known pain points you've already heard, relevant product history, or
  # anything that would help Claude understand your situation faster.
  #
  # Examples:
  # - "We ran a survey in Q1 that showed 60% of ops users are dissatisfied with reporting tools"
  # - "A previous PM tried to solve this with a Zapier integration that failed"
  # - "The main competing priority is the mobile app launch in June"

previous_research: ""             # Link or paste a summary of any prior research on this topic
existing_tools_in_use: ""         # What tools does the team currently use for this problem area?
```

---

> **Next step:** Once this file is filled in, go to your Claude chat and say:
> `"Read my input.md and start the research pipeline."`
> Claude will output a Pipeline Status Card, your segmented stakeholder list, prioritized interview queue, and scheduling pack — all pre-filled with your context.
