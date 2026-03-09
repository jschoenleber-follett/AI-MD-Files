# Weekly Blog Template Reference

## Purpose
This file contains the exact Confluence-compatible markdown template for Jason's Weekly Blog. Load this file when assembling the blog to ensure structural consistency.

## Template Structure

The blog is a Confluence page with the following metadata:
- **Title format:** `YYYY-MM-DD to YYYY-MM-DD` (Monday to Friday of the current week)
- **Space key:** `~712020ef181674768d419dbac093d4a312be81`
- **Space ID:** `2959540242`
- **Parent page ID:** `2959540281`
- **Cloud ID:** `9f0f56cb-b54d-4595-ac26-cc2d4503bd1e`

## Template

```markdown
# [Theme Title for the Week]

---

## 🔥 TL;DR

> _[2-3 sentences max. What's the ONE thing the team needs to walk away with from this post? Lead with the headline. Be direct and specific about what the team should focus on. This should read like an executive directive, not a summary.]_

---

## 📢 Announcements & Decisions

### Decisions Made

| Decision | Rationale | Impact Area | Effective |
| --- | --- | --- | --- |
| _[Decision text]_ | _[Why this decision was made]_ | _[Technology Suite / Facilities Suite / All / etc.]_ | _[Immediately / Date]_ |
|  |  |  |  |

### Heads Up

* [Announcement or important information the team needs to know. Include links where relevant.]
* [Each bullet should be substantive — context, impact, and any action needed.]

---

## 🎯 Strategic Focus This Period

**Theme:** [1-3 word theme that captures this week's strategic emphasis]

**Key Priorities (Rank Ordered):**

Status: 🟢 On Track / 🟡 At Risk / 🔴 Blocked

1. **[Priority Name]** — Owner: @[Person] — Status: 🟢 / 🟡 / 🔴

    1. [Person] - [Status note] [emoji]

2. **[Priority Name]** — Owner: @[Person] — Status: 🟢 / 🟡 / 🔴

    1. [Person] - [Status note] [emoji]

**What "Good" Looks Like This Week/Period:**

* [Specific, measurable outcome that defines success]
* [Another concrete deliverable or behavior expectation]
* [Keep these actionable — not aspirational]

---

## ❓ Questions for the Team

_Use this section to ask specific questions you need answered. Tag the relevant PM directly. Set a response deadline._

| # | Question | For | Response By | Answer |
| --- | --- | --- | --- | --- |
| 1 | _[Specific question]_ | @[Person] | [Date] | _[Leave blank for team to fill]_ |
| 2 |  |  | [Date] |  |

---

## ✅ Action Items & Asks

_Clear tasks with owners and deadlines. PMs should update status directly in this table._

| Task | Owner | Due | Status |
| --- | --- | --- | --- |
| _[Clear, specific task]_ | @[Person] | [Date] | ⬜ Not Started / 🔄 In Progress / ✅ Done |
|  |  |  |  |

---

## 📊 Numbers Worth Knowing

_Quick data points, metrics, or customer insights that inform our work. Keep it tight — context, not analysis._

* **[Metric/Insight]:** [Value or observation]
* **Source:** [Where this data came from]

---

## 🏆 Wins & Recognition

* [Specific recognition with person tagged and context on what they did well]

---

## 💬 Open Discussion / Parking Lot

_Ideas, observations, or topics that don't fit above but are worth surfacing. Team can add comments below._

* _Topic 1_
* _Topic 2_
```

## Content Guidelines

### TL;DR Section
- Lead with the most important directive or focus for the week
- Be specific: "Focus on Pendo setup and OKR benchmarking" not "Lots to do this week"
- This should work as a standalone message if the team reads nothing else

### Decisions Made Table
- Only include actual decisions, not "things we discussed"
- Rationale should explain the "why" in one sentence
- Impact Area uses suite names: Technology Suite, Facilities Suite, All, etc.

### Heads Up Bullets
- Include links to relevant documents, Jira tickets, or external articles
- If an announcement requires team action, state that explicitly
- Use Confluence mention syntax for tagging: `<custom data-type="mention" data-id="id-N">@Person Name</custom>`

### Strategic Focus Items
- Carry forward items from previous week that aren't complete
- Add new items identified from this week's intelligence
- Rank order by importance (OKR alignment, revenue impact, risk)
- Each item needs an owner and a status indicator
- Sub-items capture individual PM status updates (leave blank for them to fill)

### Questions Table
- Questions should be specific and answerable
- Always include a response deadline (typically mid-to-end of week)
- Carry forward unanswered questions from the previous blog

### Action Items Table
- Every task needs an owner and a due date
- Tasks should be specific enough that "done" is unambiguous
- Carry forward incomplete tasks from the previous blog with updated context
- Status options: ⬜ Not Started / 🔄 In Progress / ✅ Done

### Numbers Worth Knowing
- Include this section only when there are actual data points to share
- Keep it to 1-3 metrics max
- Always cite the source

### Wins & Recognition
- Tag the person being recognized
- Be specific about what they did and why it matters

### Confluence Mention Syntax
When tagging team members in Confluence markdown:
- Faisal Ghias: `@Faisal Ghias`
- Christopher Gentile: `@Christopher Gentile`
Note: Actual Confluence custom mention tags with data-ids will be handled during page creation. In the draft, use plain @mentions.

## Confluence Publishing Details

When creating the page via the Atlassian MCP tools:
- Tool: `createConfluencePage`
- Cloud ID: `9f0f56cb-b54d-4595-ac26-cc2d4503bd1e`
- Space ID: `2959540242`
- Parent ID: `2959540281`
- Content format: `markdown`
- Title: `YYYY-MM-DD to YYYY-MM-DD` (current week Monday to Friday)
