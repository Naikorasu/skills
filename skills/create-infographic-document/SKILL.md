---
name: create-infographic-document
description: Convert complex raw information into visually intuitive, structured Markdown documents using Mermaid diagrams, tables, callouts, and shields.io metadata badges. Use when user needs architectural overviews, process guides, onboarding docs, or technical documentation that is detailed yet highly scannable.
---

# Create Infographic Document

Converts complex raw information into visually intuitive, structured, and highly scannable Markdown documents using Mermaid diagrams, tables, callouts, and shields.io metadata badges.

This skill should be used when the user needs to write documentation, architectural overviews, process guides, or onboarding docs that are detailed yet simple to consume.

## Variables

The user must provide (or the agent must request) these 3 key inputs before generating the document:

- `SOURCE_INFO`: Raw text, technical notes, URLs, or concept outlines.
- `DOC_OBJECTIVE`: The primary goal or purpose of the document.
- `TARGET_AUDIENCE`: The intended reader (e.g., C-Level, Junior Dev, General Public).
- `OWNER`: The creator of the document (e.g., Github User, User who initiate to create this document).

## Instructions

When generating an infographic-style Markdown document, strictly follow these structural and formatting principles:

### 1. Document Header & Metadata

Place dynamic **shields.io** badges at the top indicating Status, Version, Audience, and Estimated Read Time. Follow immediately with a blockquote summary containing the 3 input variables and last updated date.

```markdown
# [Title of the Document]

![Status](https://img.shields.io/badge/Status-Active-brightgreen)
![Version](https://img.shields.io/badge/Version-1.0.0-blue)
![Owner](https://img.shields.io/badge/Owner-[OWNER]-yellow)
![Audience](https://img.shields.io/badge/Target-[TARGET_AUDIENCE]-orange)
![Read Time](https://img.shields.io/badge/Read_Time-5_min-green)

> **📌 DOCUMENT METADATA**
> - **Objective:** [DOC_OBJECTIVE]
> - **Audience:** [TARGET_AUDIENCE]
> - **Source:** [SOURCE_INFO]
> - **Last Updated:** [DATE]
```

### 2. Tone & Writing Style

- Keep paragraphs short (maximum 2–3 sentences).
- Avoid wall-of-text explanations — always prefer tables, bullet lists, or visual diagrams.
- Tailor language complexity and technical depth directly to `TARGET_AUDIENCE`.

### 3. Visual First (Mermaid Diagrams)

Always include at least one **Mermaid.js** diagram near the top for instant visual understanding. Match the diagram type to the information objective:

| Diagram Type | Best For |
|:---|---:|
| `flowchart TD` / `flowchart LR` | Workflows & Logic |
| `erDiagram` / architecture nodes | Entity / System Design |
| `sequenceDiagram` / `timeline` | Timeline / Sequences |
| `mindmap` | Categorization / Taxonomy |

### 4. Advanced Markdown Formatting

Utilize full Markdown features based on [Markdown Guide Cheat Sheet](https://www.markdownguide.org/cheat-sheet/):

- **Data Tables:** Align columns properly (`:---`, `:---:`, `---:`).
- **Callout Blocks:** Use `>` blockquotes formatted with contextual emojis (`> 💡 **NOTE:**`, `> ⚠️ **WARNING:**`, `> 🚀 **TIP:**`).
- **Checklists:** Use task lists (`- [x]`, `- [ ]`) for implementation steps or checklists.
- **Selective Emojis:** Use emojis only as section indicators or structural anchors (e.g., 🎯, 📊, ⚙️), not for decoration.

## Output Template Structure

Follow this structure for the final output:

```markdown
# [Catchy & Clear Title]

[Shield Badges]

[Metadata Blockquote]

---

## 💡 Executive Summary
[2-3 punchy sentences summarizing the core message]

---

## 📊 Visual Overview

```mermaid
[Relevant Mermaid Diagram]
```

---

## 🔑 Core Components Breakdown

| Component | Function | Key Detail |
| :--- | :---: | :--- |
| **Item A** | Purpose A | Specs/Details |
| **Item B** | Purpose B | Specs/Details |

> 💡 **KEY TAKEAWAY:** [Crucial insight for the reader]

---

## 🔄 Relationship & Architecture

```mermaid
[ERD / Sequence Diagram if applicable]
```

---

## 🚀 Step-by-Step Action Plan

### Implementation Phase 001 - The Simple Title of Implementaion Phase 1 

- GOAL-001: Description of the goal of Implementation Phase 001

| Task | Description | Completed | Date |
|------|-------------|-----------|------|
| TASK-001 | description |  |  |


---

## 📝 Conclusion & Next Steps
1. **Action 1:** [Immediate next step]
2. **Action 2:** [Follow-up item]
```

## Examples

### User Prompt

> Please create an infographic document about our new microservice deployment pipeline based on these raw notes: [deployment notes]. Objective is onboarding new devops engineers. Audience is Junior DevOps engineers.

### Agent Execution

1. Extract variables: `SOURCE_INFO`, `DOC_OBJECTIVE`="DevOps Onboarding", `TARGET_AUDIENCE`="Junior DevOps Engineers".
2. Render badges and metadata block.
3. Build a `flowchart LR` Mermaid diagram illustrating CI/CD pipeline steps.
4. Format deployment steps using task lists and environment configs into clean tables.
5. Highlight critical security notices using warning callout boxes (`> ⚠️ **CRITICAL:**`).
