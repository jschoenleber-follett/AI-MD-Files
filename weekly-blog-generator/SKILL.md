---
name: weekly-blog-generator
description: "Generate Jason's Weekly Blog for his product team by orchestrating intelligence gathering from Glean and carry-forward review from the previous Confluence blog, then assembling a complete blog post and publishing it to Confluence after review. This is the primary entry point for the full weekly blog workflow — it automatically invokes the weekly-intel-gather and weekly-blog-review skills. Trigger whenever the user asks to write the weekly blog, create this week's blog, generate the weekly update, publish the weekly blog, or draft the weekly post. Also trigger on: 'weekly blog,' 'team blog,' 'Monday blog,' 'write the blog,' 'blog time,' 'weekly update for the team,' or any indication that the user wants to produce their regular team communication. Even casual requests like 'let's do the blog' or 'time for the weekly' should fire this skill."
---

# Weekly Blog Generator

## Purpose

You are a **weekly communications orchestrator** responsible for producing Jason's Weekly Blog — the primary written communication channel between Jason and his product team (Faisal Ghias and Christopher Gentile).

This skill coordinates the full blog creation pipeline: gathering intelligence from the past week, reviewing carry-forward items from the previous blog, assembling the content into the established template, and publishing to Confluence after Jason's review.

The blog must feel like Jason wrote it — direct, structured, operator-minded, and grounded in K-12 product reality. It should make the team's priorities crystal clear and ensure nothing falls through the cracks.

## When to Use This vs. Other Skills

| Use this skill when… | Use weekly-intel-gather when… | Use weekly-blog-review when… |
|---|---|---|
| You want the full blog creation pipeline | You only need raw intelligence from the past week | You only need carry-forward items from the last blog |
| The user says "write my weekly blog" | The user says "what happened last week" | The user says "what's still open from last week" |

## Assumptions

- Both Glean and Confluence MCP tools are connected and accessible
- The `weekly-intel-gather` and `weekly-blog-review` skills are installed and available
- Jason's Writing Style Guide skill (`jasons-writing-style-guide`) may be available for tone calibration — use if present, skip gracefully if not
- The user will review the draft before publishing (Phase 4 is mandatory)
- Confluence page creation requires: Cloud ID `9f0f56cb-b54d-4595-ac26-cc2d4503bd1e`, Space ID `2959540242`, Parent ID `2959540281`

## Critical Architecture Note

**Phase 1 and Phase 2 MUST run as Task subagents, NOT inline.** Previous versions tried to execute all Glean searches and Confluence reads in the main conversation context. This failed because Glean search results return 50-100KB of raw data per query, exhausting the context window before assembly could begin.

The fix: Launch Phase 1 and Phase 2 as parallel Task subagents. Each subagent gets its own context window, does the heavy data processing internally, and returns only a concise structured summary back to the orchestrator for assembly.

## Orchestration Model

```
Phase 1: Intel Gathering ──┐
    (Task subagent)         ├── run in PARALLEL
Phase 2: Blog Review ──────┘
    (Task subagent)
         │
         ↓ both return concise structured summaries
Phase 3: Assembly (this skill's core logic)
    ↓ produces: blog draft
Phase 4: User Review
    ↓ user approves or requests changes
Phase 5: Confluence Publishing
    ↓ creates: new Confluence page
```

---

## Phase 1: Intelligence Gathering (Task Subagent)

**IMPORTANT: Launch this as a Task subagent using the Task tool with `subagent_type: "general-purpose"`.**

Calculate the previous work week date range first (if today is Monday, previous week is 7-3 days ago). Then launch the subagent with this prompt:

```
You are gathering weekly intelligence for Jason Schoenleber's Weekly Blog.

Date range: [Monday YYYY-MM-DD] through [Friday YYYY-MM-DD]

Use Glean chat (mcp__glean_default__chat) to run these 4 synthesis queries. Run them in parallel where possible:

1. "What decisions did Jason Schoenleber make or communicate between [Monday] and [Friday]? What announcements did he share? What action items or tasks did he assign to Faisal Ghias, Christopher Gentile, or others? Include the source and any deadlines mentioned. Focus on ITAM, Facilities Suite, and Resource Manager portfolio work."

2. "What product strategy, GTM, competitive, or sales enablement work happened at Follett Software between [Monday] and [Friday] related to ITAM, Facilities Suite (Schedules, Work Orders, Drawings, Events Registration), or Resource Manager? Include pipeline data, pricing discussions, pathway or bundle work, Incident IQ or FMX competitive mentions, and marketing campaign updates."

3. "What metrics, data points, NPS results, Pendo analytics, pipeline numbers, or performance data were shared at Follett Software between [Monday] and [Friday]? Also identify team wins, recognition, completed milestones, or successful releases."

4. "What AI strategy, platform unification, Glean adoption, or cross-functional alignment work happened at Follett Software between [Monday] and [Friday]? Include unified platform, Follett 360, AI assistants, ticket triage, automation, OKR updates, or Pendo instrumentation work."

After getting all responses, organize the findings into this CONCISE structured format (keep the entire output under 3000 words):

## Decisions Made
| Decision | Rationale | Impact Area | Effective |
(one row per decision, max 5)

## Announcements & Heads Up
- (bullet per announcement, max 6 bullets, 1-2 sentences each)

## Strategic Focus Items
| Priority | Item | Owner | Status Signal |
(rank ordered, max 6)

## Questions Raised
| # | Question | For | Suggested Response By |
(max 5)

## Action Items (Prioritized)
| Priority | Task | Owner | Due |
(P1-P4, max 8)

## Metrics & Numbers Worth Knowing
- (max 3, with source)

## Wins & Recognition
- (max 4)

## Open / Parking Lot
- (max 3)

DO NOT include raw Glean response data. Only include the synthesized, categorized output.
```

---

## Phase 2: Previous Blog Review (Task Subagent)

**IMPORTANT: Launch this as a Task subagent IN PARALLEL with Phase 1, using the Task tool with `subagent_type: "general-purpose"`.**

```
You are reviewing Jason Schoenleber's most recent Weekly Blog from Confluence to extract carry-forward items for the next blog.

Step 1: Find the most recent blog page.
Use searchConfluenceUsingCql with:
- cloudId: "9f0f56cb-b54d-4595-ac26-cc2d4503bd1e"
- cql: space = "~712020ef181674768d419dbac093d4a312be81" AND type = "page" AND title ~ "202" ORDER BY created DESC
- limit: 3

Pick the first result that has a date-range title (format: YYYY-MM-DD to YYYY-MM-DD). Skip "MySpace" (ID 2959540281).

Step 2: Read the full blog content.
Use getConfluencePage with the page ID found above, contentFormat: "markdown", cloudId: "9f0f56cb-b54d-4595-ac26-cc2d4503bd1e".

Step 3: Extract carry-forward items. Parse the blog and identify:

A. UNANSWERED QUESTIONS: From the Questions table, find rows where the Answer column is empty or has only a partial response.

B. INCOMPLETE ACTION ITEMS: From the Action Items table, find rows where Status is NOT "Done" or "✅". Note if overdue.

C. IN-PROGRESS STRATEGIC FOCUS ITEMS: Items with 🟡 or 🔴 status, or 🟢 with caveats.

D. DECISIONS NEEDING FOLLOW-UP: From Announcements/Decisions that mention future actions.

E. PARKING LOT ITEMS: Any substantive parking lot items (skip empty bullets).

Step 4: Return this CONCISE structured output (keep under 1500 words):

## Previous Blog
- Title: [date range]
- Page ID: [id]

## Unanswered Questions (Carry Forward)
| # | Question | Assigned To | Original Deadline |
(only unanswered ones)

## Incomplete Action Items (Carry Forward)
| Task | Owner | Original Due | Last Status | Overdue? |
(only incomplete ones)

## Strategic Focus Items Still Active
| Priority | Item | Owner | Last Status | Recommendation |
(keep/elevate/drop)

## Decisions Needing Follow-Up
- (bullets, only if applicable)

## Parking Lot
- (bullets, only if substantive)

## Summary
- Total carry-forward items: [count]

DO NOT include the full blog content in your response. Only include the extracted carry-forward items.
```

---

## Phase 3: Blog Assembly

Once both subagents return, assemble the blog. Read the blog template from `references/blog-template.md` for the exact structure and Confluence publishing details.

### 3a. Determine Blog Metadata

- **Title:** This week's Monday to Friday in `YYYY-MM-DD to YYYY-MM-DD` format
- **Theme:** Identify the dominant theme from the intel report (e.g., "Analytics and AI", "Cross-Team Alignment", "Release Execution", "GTM Readiness")

### 3b. Write the TL;DR

Synthesize the intel report and carry-forward items into 2-3 sentences that:
- Lead with the most important directive or focus
- Are written in Jason's voice — direct, specific, operator-minded
- Could stand alone as the entire communication

### 3c. Populate Announcements & Decisions

- Pull decisions from the intel report's Decisions table
- Pull heads-up items from the intel report's Announcements
- Each decision needs: text, rationale, impact area, effective date

### 3d. Build Strategic Focus Section

1. Start with carry-forward strategic items (still in progress)
2. Add new strategic items from the intel report
3. Rank order by: OKR alignment, revenue/retention impact, risk level
4. Set the Theme based on what dominates
5. Write "What Good Looks Like" — 4-6 specific measurable outcomes

### 3e. Populate Questions Table

1. Carry forward unanswered questions (update deadlines, mark as "carried forward")
2. Add new questions from the intel report

### 3f. Populate Action Items Table

1. Carry forward incomplete items (update due dates, mark as "carried forward")
2. Add new action items from intel report (every item needs owner + due date)

### 3g. Populate Remaining Sections

- **Numbers Worth Knowing:** Only if intel surfaced specific metrics. Skip if empty.
- **Wins & Recognition:** Pull from intel. If thin, note completed strategic items.
- **Parking Lot:** Carry forward + new topics that don't fit above.

### 3h. Voice and Tone Check

Review against Jason's style:
- **Direct and specific** — no corporate fluff
- **Operator-minded** — connects to retention, ARR, adoption, GTM
- **K-12 grounded** — real workflows, not abstractions
- **Decisive** — states what the team should do
- **Candid** — if at risk, say so

### 3i. Output the Draft

Present the complete blog to the user with:
- Brief summary of sources ("Found N carry-forward items, N new action items, N decisions")
- The complete blog in template format
- Flags for any judgment calls or thin sections

---

## Phase 4: User Review

Present the draft and ask for feedback:

"Here's the draft for this week's blog ([Monday] to [Friday]). A few notes:
- [Judgment calls or thin sections]
- [Items you weren't sure about]

Review and let me know what changes you'd like before I publish."

Iterate until Jason approves.

---

## Phase 5: Confluence Publishing

Once approved, publish to Confluence:

1. Use `createConfluencePage`:
   - Cloud ID: `9f0f56cb-b54d-4595-ac26-cc2d4503bd1e`
   - Space ID: `2959540242`
   - Parent ID: `2959540281`
   - Title: `YYYY-MM-DD to YYYY-MM-DD`
   - Content format: `markdown`
   - Body: approved blog content

2. Confirm publication and provide the page URL.

3. Remind: "Published. Team should review within 24 hours and respond to tagged questions by stated deadlines."

---

## Quality Checklist (Before Publishing)

- [ ] Blog title follows `YYYY-MM-DD to YYYY-MM-DD` format with correct dates
- [ ] TL;DR is 2-3 sentences and could stand alone
- [ ] All carry-forward items accounted for (included or explicitly dropped)
- [ ] Strategic focus items rank-ordered with owners and status
- [ ] Every action item has owner and due date
- [ ] Every question has "For" person and "Response By" date
- [ ] Content written in Jason's voice
- [ ] No sections with only placeholder text
- [ ] Draft reviewed and approved by Jason before publishing

## Adaptation Rules

- **If the user provides specific topics or priorities:** Incorporate into TL;DR and Strategic Focus, adjusting rank order.
- **If Glean returns limited results:** Lean on carry-forward items and ask Jason to supplement. Don't fabricate.
- **If the user wants to skip a section:** Remove it rather than including placeholders.
- **If the user wants to run only assembly** (already has data): Accept their data and go to Phase 3.
- **If there's no previous blog:** Skip Phase 2 subagent. Note "First blog post — no carry-forward items."
