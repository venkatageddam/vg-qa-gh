---
name: evaluate-us
description: Evaluate a user story by scoring it across 7 quality categories (Core Structure, Functional Requirements, Validation Specifications, UI/UX Requirements, API Requirements, DB Requirements, Non-Functional Requirements). Called by user-story-evaluation-agent. Usage: /evaluate-us <work-item-id> [excluded: <categories>]
argument-hint: <work-item-id> [excluded: <categories>]
user-invocable: true
allowed-tools: Read, Write, mcp__rpdevops__*
---

# User Story Evaluation Skill

You are evaluating a user story for quality and completeness. Score it across the applicable categories below.

## Inputs

- **WORK_ITEM_CONTENT**: The full text of the work item (title, description, acceptance criteria, test data)
- **EXCLUDED_CATEGORIES**: Categories to skip (or `"None"` to evaluate all)
- **WORK_ITEM_ID**: The TFS/Azure DevOps work item ID
- **TODAY**: Today's date in YYYY-MM-DD format

---

## Scoring Categories

Score each applicable category from **1–10** using the rubric below.
Skip any category listed in EXCLUDED_CATEGORIES.

| # | Category | What to Evaluate |
|---|---|---|
| 1 | **Core Structure** | Title clarity, description context, business value, user persona, story format |
| 2 | **Functional Requirements** | All user actions described, workflows clear, edge cases mentioned |
| 3 | **Validation Specifications** | Acceptance criteria specific, testable, measurable, complete |
| 4 | **UI/UX Requirements** | UI elements described, UX flows documented, mockup references present |
| 5 | **API Requirements** | Endpoints identified, request/response formats described, auth specified |
| 6 | **DB Requirements** | Data model changes identified, schema impact described |
| 7 | **Non-Functional Requirements** | Performance, security, accessibility, scalability criteria present |

---

## Scoring Rubric

| Score | Meaning |
|---|---|
| 9–10 | Complete — fully defined, no gaps |
| 7–8 | Good — minor gaps, actionable |
| 5–6 | Partial — significant gaps, needs work |
| 3–4 | Poor — major sections missing |
| 1–2 | Inadequate — insufficient to proceed |

---

## Output Format

Generate the evaluation report as a Markdown file saved to `output/evaluations/US<WORK_ITEM_ID>-evaluation-<TODAY>.md`:

```markdown
# User Story Evaluation Report
**Work Item:** #<WORK_ITEM_ID> — <Title>
**Evaluated:** <TODAY>
**Evaluator:** Claude User Story Evaluation Agent

## Overall Score: [X.X / 10] — [READY / NEEDS WORK / NOT READY]

## Category Scores

| Category | Score | Status | Key Gaps |
|---|---|---|---|
| Core Structure | X/10 | ✅/⚠️/❌ | ... |
| Functional Requirements | X/10 | ✅/⚠️/❌ | ... |
| Validation Specifications | X/10 | ✅/⚠️/❌ | ... |
| UI/UX Requirements | X/10 | ✅/⚠️/❌ | ... |
| API Requirements | X/10 | ✅/⚠️/❌ | ... |
| DB Requirements | X/10 | ✅/⚠️/❌ | ... |
| Non-Functional Requirements | X/10 | ✅/⚠️/❌ | ... |

## Detailed Findings

### [Category Name]
**Score: X/10**
**Strengths:** ...
**Gaps:** ...
**Recommendations:** ...

## Readiness Verdict
[GO / CONDITIONAL GO / NOT READY] with rationale

## Top 3 Actions Required
1. ...
2. ...
3. ...
```

---

## Readiness Thresholds

| Overall Score | Verdict |
|---|---|
| ≥ 8.0 | ✅ READY — proceed to sprint |
| 6.0–7.9 | ⚠️ CONDITIONAL GO — address gaps before sprint start |
| < 6.0 | ❌ NOT READY — return to product owner |
