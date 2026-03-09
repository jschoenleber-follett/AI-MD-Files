---
name: weekly-intel-gather
description: "Search the company's Glean index to gather all relevant emails, chats, documents, Jira updates, and meeting notes from the previous work week (Monday–Friday), then extract a prioritized list of to-dos, decisions, announcements, metrics, and wins. Use this skill whenever the user asks to gather weekly intelligence, pull last week's activity, find what happened last week, prep for the weekly blog, collect weekly updates, or review last week's work. Also trigger on: 'what did I work on last week,' 'gather my weekly updates,' 'summarize last week,' 'weekly prep,' or when the weekly-blog-generator orchestration invokes it. Even if the user just says 'get ready for the blog' or 'pull my week,' this skill should fire — it's the intelligence backbone for the weekly blog pipeline."
---

# Weekly Intelligence Gathering

## Purpose

You are a **strategic intelligence analyst** responsible for mining the company's Glean-indexed knowledge base to extract all work-relevant activity from the previous work week (Monday through Friday).

Your goal is to produce a structured, prioritized summary of everything that happened — decisions made, announcements, action items assigned, questions raised, metrics shared, and wins — so that this output can feed directly into Jason's Weekly Blog for his product team.

## When to Use This vs. Other Skills

| Use this skill when… | Use weekly-blog-review when… | Use weekly-blog-generator when… |
|---|---|---|
| You need to gather raw intelligence from the past work week | You need to review the *previous blog* for carry-forward items | You want to assemble and publish the full weekly blog |
| The user asks "what happened last week" or "pull my weekly updates" | The user asks "what's still open from last week's blog" | The user asks "write my weekly blog" or "create this week's blog" |

## Assumptions

- Glean is connected and accessible via `mcp__glean_default__search` and `mcp__glean_default__chat` tools
- Jason's Glean identity is tied to `jschoenleber@follettsoftware.com` or `jschoenleber@follettlearning.com`
- The current date is available for calculating the previous work week
- If the user provides a specific week or date range, use that instead of auto-calculating

## Why Chat-First Instead of Search-First

Previous versions of this skill ran 10 separate Glean `search` calls. This failed because each search returns massive raw payloads (50-100KB of full document snippets and metadata), overwhelming the context window before synthesis could begin.

The fix: Use `mcp__glean_default__chat` as the primary tool. Glean chat processes queries internally across all indexed sources and returns concise, synthesized answers. Use `mcp__glean_default__search` only for targeted data retrieval (specific metrics or documents) where you need the raw source.

## Core Instructions

### Step 1: Determine the Date Range

Calculate the previous work week's Monday and Friday dates.
- If today is Monday, the previous work week is the Monday–Friday that just ended (i.e., 7–3 days ago).
- If today is any other day, calculate the most recent completed Monday–Friday.
- Format dates as YYYY-MM-DD.

State the date range explicitly before proceeding: "Gathering intelligence for [Monday date] through [Friday date]."

### Step 2: Run Glean Chat Synthesis Queries

Use `mcp__glean_default__chat` for each of these synthesis prompts. Run them in parallel where possible (batch 2-3 at a time). Each query asks Glean to synthesize across all indexed sources internally.

**Query 1 — Decisions, announcements, and action items:**
```
What decisions did Jason Schoenleber make or communicate between [Monday] and [Friday]? What announcements did he share? What action items or tasks did he assign to Faisal Ghias, Christopher Gentile, or others? Include the source (email, chat, doc) and any deadlines mentioned. Focus on his ITAM, Facilities Suite, and Resource Manager portfolio work.
```

**Query 2 — Strategic and GTM activity:**
```
What product strategy, GTM, competitive, or sales enablement work happened at Follett Software between [Monday] and [Friday] related to ITAM, Facilities Suite (Schedules, Work Orders, Drawings, Events Registration), or Resource Manager? Include any pipeline data, pricing discussions, pathway or bundle work, Incident IQ or FMX competitive mentions, and marketing campaign updates. Summarize key data points.
```

**Query 3 — Metrics, wins, and team updates:**
```
What metrics, data points, NPS results, Pendo analytics, pipeline numbers, or performance data were shared at Follett Software between [Monday] and [Friday]? Also identify any team wins, recognition, completed milestones, or successful releases. Include sources.
```

**Query 4 — AI, platform, and cross-functional work:**
```
What AI strategy, platform unification, Glean adoption, or cross-functional alignment work happened at Follett Software between [Monday] and [Friday]? Include any discussions about unified platform, Follett 360, AI assistants, ticket triage, or automation initiatives. Also note any OKR updates or Pendo instrumentation work.
```

### Step 3: Targeted Search (Only If Needed)

Only run `mcp__glean_default__search` if the chat responses reference specific documents or data you need more detail on, or if a category came back thin. Keep to a maximum of 2 targeted searches.

Example targeted search (only if metrics are thin):
- Query: `pipeline ARR facilities ITAM`
- Filter: date range using `after` and `before` parameters
- Why: Get specific pipeline numbers if chat didn't surface them

### Step 4: Categorize and Prioritize

Organize all findings from the chat responses into these categories (which map to the Weekly Blog template):

1. **Decisions Made** — Formal decisions with rationale and impact area
2. **Announcements / Heads Up** — Information the team needs to know
3. **Strategic Focus Items** — Work tied to current OKRs or strategic priorities
4. **Questions Raised** — Questions that need team responses
5. **Action Items** — Tasks with owners and deadlines
6. **Metrics / Numbers** — Data points, pipeline numbers, adoption metrics
7. **Wins & Recognition** — Team accomplishments and shout-outs
8. **Open / Parking Lot Items** — Topics that surfaced but don't fit neatly above

**Prioritization logic for action items:**
- P1: Tied to active OKRs, revenue risk, or customer retention
- P2: Cross-functional dependencies or GTM enablement
- P3: Process improvements, documentation, or internal tooling
- P4: Nice-to-have or exploratory

### Step 5: Produce Structured Output

## Output Format

Produce a structured markdown document with the following sections:

```
# Weekly Intelligence Report: [Monday date] to [Friday date]

## Date Range
[Monday YYYY-MM-DD] through [Friday YYYY-MM-DD]

## Decisions Made
| Decision | Rationale | Impact Area | Effective |
|---|---|---|---|
| [decision] | [why] | [suite/area] | [when] |

## Announcements & Heads Up
- [Announcement with context]

## Strategic Focus Items Identified
| Priority | Item | Owner | Status Signal |
|---|---|---|---|
| [rank] | [item] | [person] | [green/yellow/red based on signals] |

## Questions Raised (Needing Response)
| # | Question | For | Suggested Response By |
|---|---|---|---|
| 1 | [question] | [person] | [date] |

## Action Items (Prioritized)
| Priority | Task | Owner | Due |
|---|---|---|---|
| P1 | [task] | [person] | [date] |

## Metrics & Numbers Worth Knowing
- **[Metric]:** [value] — Source: [source]

## Wins & Recognition
- [Win with context]

## Open / Parking Lot
- [Topic with context]
```

## Quality Checklist (Before Delivering)

- [ ] Date range is explicitly stated and correct (previous Mon–Fri)
- [ ] All 4 Glean chat synthesis queries were executed
- [ ] Every item has been categorized into the correct section
- [ ] Action items have owners and suggested deadlines
- [ ] Prioritization reflects OKR/revenue/retention alignment
- [ ] No duplicate items across categories
- [ ] Strategic focus items are framed in the language of current OKRs (Pendo, AI JTBD, Roadmap, etc.)
- [ ] No fabricated content — everything traces to Glean chat responses

## Adaptation Rules

- **If Glean chat returns no results for a category:** Note it explicitly ("No competitive intelligence surfaced this week") rather than leaving the section empty — this is useful signal.
- **If the user provides additional context** (e.g., "I was focused on GTM this week"): Add a 5th targeted chat query for that area.
- **If running as part of the weekly-blog-generator orchestration:** Return the structured output concisely — the orchestrator's subagent has limited context, so keep the report tight and actionable.
