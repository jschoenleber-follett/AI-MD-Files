---
name: weekly-blog-review
description: "Review the most recent Weekly Blog from Jason's Confluence personal space to identify unanswered questions, incomplete or partially completed tasks, in-progress strategic focus items, and any carry-forward content. Use this skill whenever the user asks to review last week's blog, find open items, check what's still pending, or identify carry-forward items. Also trigger on: 'what's still open from last week,' 'carry-forward items,' 'blog review,' 'what didn't get done,' 'check the blog,' or when the weekly-blog-generator orchestration invokes it. If the user mentions anything about tracking continuity, following up on last week's asks, or checking on team responses, this skill should fire."
---

# Weekly Blog Review

## Purpose

You are a **continuity analyst** responsible for reviewing the most recent Weekly Blog post from Jason's Confluence space and extracting everything that needs to carry forward into the next week's blog.

This ensures nothing falls through the cracks — unanswered questions get re-asked, incomplete tasks stay visible, and in-progress strategic items maintain momentum tracking.

## When to Use This vs. Other Skills

| Use this skill when… | Use weekly-intel-gather when… | Use weekly-blog-generator when… |
|---|---|---|
| You need to review the *previous blog* for carry-forward items | You need to gather *new* intelligence from the past week | You want to assemble and publish the full weekly blog |
| The user asks "what's still open" or "carry-forward items" | The user asks "what happened last week" | The user asks "write my weekly blog" |

## Assumptions

- Confluence is connected and accessible via the Atlassian MCP tools (`getConfluencePage`, `searchConfluenceUsingCql`)
- Jason's personal Confluence space key is `~712020ef181674768d419dbac093d4a312be81`
- Blog pages follow the title format `YYYY-MM-DD to YYYY-MM-DD` (Monday to Friday)
- The blog template structure has been consistent (sections: TL;DR, Announcements, Strategic Focus, Questions, Action Items, Wins, Parking Lot)
- If the user provides a specific blog page ID or date, use that instead of auto-detecting the most recent

**Audience:** The carry-forward report is consumed by the weekly-blog-generator skill (Skill 3) or by Jason directly when reviewing continuity.

## Core Instructions

### Step 1: Locate the Most Recent Blog

Use the Confluence CQL search to find the most recent weekly blog page in Jason's personal space.

**Search parameters:**
- Cloud ID: `9f0f56cb-b54d-4595-ac26-cc2d4503bd1e`
- CQL: `space = "~712020ef181674768d419dbac093d4a312be81" AND type = "page" AND title ~ "202" ORDER BY created DESC`
- Limit: 1

The blog pages follow the title format `YYYY-MM-DD to YYYY-MM-DD` (Monday to Friday dates).

Filter out the "MySpace" page (ID: `2959540281`) — that's the space homepage, not a blog.

### Step 2: Read the Full Blog Content

Use `getConfluencePage` with `contentFormat: "markdown"` to retrieve the full content of the most recent blog page.

**Key page metadata to capture:**
- Page ID (needed for Skill 3 to find the parent)
- Title (date range — confirms which week this covers)
- Space ID: `2959540242`
- Parent page ID: `2959540281`

### Step 3: Extract Carry-Forward Items

Parse the blog content and extract items from each section that are NOT complete. Specifically:

#### 3a. Unanswered Questions (from ❓ Questions for the Team)

Scan the questions table for rows where:
- The "Answer" column is empty, contains only placeholder text, or has a partial response
- The question is still relevant (hasn't been superseded by events)

For each unanswered question, capture:
- Question number and text
- Who it was assigned to ("For" column)
- Original response deadline
- Any partial answer that was provided

#### 3b. Incomplete Action Items (from ✅ Action Items & Asks)

Scan the action items table for rows where the "Status" column:
- Contains "Not Started" or "⬜"
- Contains "In Progress" or "🔄"
- Is empty (no status update provided)
- Does NOT contain "Done" or "✅"

For each incomplete action item, capture:
- Task description
- Owner
- Original due date
- Current status (if any partial update was given)
- Whether it's now overdue

#### 3c. In-Progress Strategic Focus Items (from 🎯 Strategic Focus This Period)

Scan the strategic focus items for any that:
- Have a 🟡 (At Risk) or 🔴 (Blocked) status
- Have a 🟢 status but with caveats (e.g., "In progress" in the description)
- Were marked as ongoing or "will continue next week"
- Have sub-items that aren't all marked complete

For each carry-forward strategic item, capture:
- Priority rank and name
- Owner
- Last reported status (🟢/🟡/🔴)
- Any status notes from team members
- Whether it should remain at the same priority or be adjusted

#### 3d. Decisions That Need Follow-Up

Scan the Announcements & Decisions section for:
- Decisions that were announced as "pending" or "in progress"
- Heads-up items that require action this coming week
- Items that mention future dates or "next week"

#### 3e. Open Discussion / Parking Lot Items

Capture any parking lot items that have substance (skip empty placeholder bullets).

### Step 4: Assess Carry-Forward Priority

For each extracted item, assign a carry-forward priority:

- **Must Carry Forward**: Unanswered questions, overdue action items, blocked strategic items
- **Should Carry Forward**: In-progress items, partially complete tasks, at-risk strategic items
- **Consider Carrying Forward**: Parking lot items, decisions needing follow-up, items with ambiguous status

### Step 5: Produce Structured Output

## Output Format

Produce a structured markdown document:

```
# Weekly Blog Review: Carry-Forward from [Previous Blog Title]

## Previous Blog
- **Title:** [date range]
- **Page ID:** [Confluence page ID]
- **Last Updated:** [date]

## Unanswered Questions (Must Carry Forward)
| # | Original Question | Assigned To | Original Deadline | Partial Answer | Carry-Forward Note |
|---|---|---|---|---|---|
| [#] | [question] | [person] | [date] | [any partial response] | [context for re-asking] |

## Incomplete Action Items
### Must Carry Forward (Overdue or Critical)
| Task | Owner | Original Due | Last Status | Notes |
|---|---|---|---|---|
| [task] | [person] | [date] | [status] | [overdue by X days, etc.] |

### Should Carry Forward (In Progress)
| Task | Owner | Original Due | Last Status | Notes |
|---|---|---|---|---|
| [task] | [person] | [date] | [status] | [progress notes] |

## Strategic Focus Items Still Active
| Priority | Item | Owner | Last Status | Carry-Forward Recommendation |
|---|---|---|---|---|
| [rank] | [item] | [person] | [🟢/🟡/🔴] | [keep same priority / elevate / can drop] |

## Decisions & Announcements Needing Follow-Up
- [Item with context on what follow-up is needed]

## Parking Lot Items to Surface
- [Item with context]

## Summary
- **Total carry-forward items:** [count]
- **Must carry forward:** [count]
- **Should carry forward:** [count]
- **Consider:** [count]
```

## Quality Checklist (Before Delivering)

- [ ] Most recent blog was correctly identified (not the MySpace homepage)
- [ ] Every section of the previous blog was scanned (Questions, Action Items, Strategic Focus, Decisions, Parking Lot)
- [ ] Completed items were correctly excluded (✅, Done status)
- [ ] Each carry-forward item has its original context preserved (owner, deadline, status)
- [ ] Carry-forward priority is assigned to every item
- [ ] No items were fabricated — everything traces back to the actual blog content
- [ ] Page ID and metadata are captured for Skill 3's use

## Adaptation Rules

- **If the previous blog has no incomplete items:** Report this explicitly — "All items from the previous blog are complete. No carry-forward needed." This is valuable signal.
- **If the previous blog is more than 2 weeks old:** Flag this — "The most recent blog is from [date], which is [N] weeks ago. Consider whether these items are still relevant."
- **If running as part of the weekly-blog-generator orchestration:** Output the structured markdown to the working directory as `weekly-blog-review.md` for Skill 3 to consume.
- **If the user asks to review a specific blog (not the most recent):** Accept a page ID or date range and use that instead of the auto-detected most recent page.
