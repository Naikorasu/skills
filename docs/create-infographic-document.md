# Create Infographic Document

## Purpose

The `create-infographic-document` skill turns raw information into a structured Markdown document with Mermaid diagrams, tables, callouts, and metadata badges. Use it for architectural overviews, process guides, onboarding material, and technical documentation that readers need to scan quickly.

The [skill instructions](../skills/create-infographic-document/SKILL.md) define the document format. This guide explains its inputs, workflow, and expected output.

## Installation and Requirements

Install the skill with:

```sh
npx skills add https://github.com/naikorasu/skills --skill create-infographic-document
```

Add `-g` if you want a global installation.

The agent needs access to the source information and a way to create Markdown files. The skill does not require another agent skill, a CLI renderer, or a particular agent platform. A Markdown viewer with Mermaid support is needed to display the diagrams, and network access is needed to display the external shields.io badge images.

## Required Inputs

Provide all four inputs before the document is generated:

| Input | Description |
|-------|-------------|
| `SOURCE_INFO` | Supply the raw text, technical notes, URLs, or outline that the document should explain. |
| `DOC_OBJECTIVE` | State what the document should help readers understand or accomplish. |
| `TARGET_AUDIENCE` | Identify the readers and their expected technical knowledge. |
| `OWNER` | Identify the document's creator or responsible owner. |

If an input is missing, the agent asks for it. Include an output filename in the request when the document should be saved to a particular location; the skill does not define a default destination.

## Workflow

1. The agent establishes the source information, objective, audience, and owner.
2. It writes a document header with status, version, owner, audience, and estimated reading-time badges.
3. It adds a metadata block that states the objective, audience, source, and last-updated date.
4. It places at least one Mermaid diagram near the top so that the main structure or process is immediately visible.
5. It organizes the details into short sections, tables, callouts, and checklists appropriate to the audience.
6. It finishes with an action plan and practical next steps.

The diagram type should match the information. A flowchart describes a process, an entity-relationship diagram describes data relationships, a sequence diagram describes interactions, and a mind map describes conceptual groupings.

## Expected Output

The result is a Markdown document with this structure:

| Section | Content |
|---------|---------|
| Document header. | It contains the title, shields.io badges, and metadata block. |
| Executive summary. | It explains the main point in a short introduction. |
| Visual overview. | It presents the main Mermaid diagram near the top. |
| Core components. | It describes important elements in tables and concise sections. |
| Relationships or architecture. | It explains relevant connections, using another diagram when useful. |
| Action plan. | It lists goals, tasks, completion status, and dates when applicable. |
| Conclusion and next steps. | It states what readers should do next. |

Paragraphs should remain short, normally two or three sentences. Emojis should identify meaningful sections or callouts rather than serve as decoration.

## Example Request

```text
Use create-infographic-document to explain our release process.

SOURCE_INFO: A developer opens a pull request. CI runs tests. A reviewer
approves the change. The release owner deploys to staging, checks the
application, and then approves production deployment.
DOC_OBJECTIVE: Help new developers understand the release checkpoints.
TARGET_AUDIENCE: Junior developers joining the team.
OWNER: Platform Team.

Save the document to docs/release-process.md.
```

## Review Before Sharing

Confirm that all four inputs are represented accurately, the diagrams match the source information, and the action plan is useful for the intended audience. Open the file in a Mermaid-capable Markdown viewer to check diagram syntax and badge rendering. A generated Markdown file should not be described as visually verified until it has been viewed.

The skill creates documentation, not an application interface or a standalone infographic image.
