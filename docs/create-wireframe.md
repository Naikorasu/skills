# Create Wireframe

## Purpose

The `create-wireframe` skill creates editable UI wireframes in wiremd Markdown and renders them to HTML. Use it for screen sketches, mockups, connected screen flows, and revisions to an existing wiremd file.

The [skill instructions](../skills/create-wireframe/SKILL.md) define the workflow and syntax guidance. This guide explains how to request a wireframe and use its deliverables.

## Installation and Requirements

Install the skill with:

```sh
npx skills add https://github.com/naikorasu/skills --skill create-wireframe
```

Add `-g` if you want a global installation.

Rendering requires Node.js, npm, and the `wiremd` CLI supplied by `@eclectic-ai/wiremd`. The agent first checks the installed CLI with `wiremd --version`. If it is missing, the installation command is:

```sh
npm install -g @eclectic-ai/wiremd
```

The agent must obtain permission for a global installation when required by the host. It then checks the version and reads `wiremd --help` for the installed interface. A browser is optional for visual review. Claude Code, a separate repository checkout, and additional agent skills are not required.

## Inputs

Provide enough information to establish the screen's purpose and content:

| Input | Description |
|-------|-------------|
| Purpose and audience. | Explain what users should accomplish and who they are. |
| Screen content. | Describe navigation, headings, fields, actions, lists, tables, and other important elements. |
| Scope. | State whether the request covers one screen or a connected multi-page flow. |
| States and references. | Provide an existing interface or describe relevant empty, loading, error, and success states. |
| Destination. | Specify where the Markdown and rendered HTML should be saved. |
| Style. | Specify a supported wiremd style when the default is unsuitable. |

The agent asks when the scope is unclear. It uses the current working directory when no destination is supplied. This skill defaults to the `clean` style, although the CLI's own default is `sketch`.

## Workflow

1. The agent establishes the purpose, audience, content, scope, and destination.
2. If an existing interface is supplied, it records the visible layout and meaningful states rather than implementation details.
3. It verifies the CLI and uses the installed help as the reference for supported options.
4. It writes the Markdown in visual reading order, starting with navigation and continuing through the main content and supplementary states.
5. It renders the Markdown to HTML and checks the command's exit status and output.
6. When browser access is available, it reviews the rendered result and corrects material problems.
7. It reports the source and HTML paths, style, preview method, and any unverified visual details.

## Rendering and Preview

Render one screen with:

```sh
wiremd screen.md -o screen.html --style clean
```

For a local live preview, use:

```sh
wiremd screen.md --serve --watch --style clean
```

Open the address printed by the CLI only when the browser can reach the same machine. If the agent runs on a remote or isolated host, use the generated HTML file instead of assuming its localhost preview is accessible.

For a directory containing several screens, use:

```sh
wiremd ./screens --style clean
wiremd ./screens --serve --watch --style clean
```

The directory server supports linked multi-page previews. Verify links before describing a static HTML export as a standalone clickable prototype, because exported links can retain `.md` targets.

## Expected Output

The editable source is a `.md` file. The rendered deliverable is an HTML file or a set of HTML pages for a multi-page request. The agent should preserve the Markdown source so that later changes remain easy to make.

Shared partials can reduce duplication in connected screens. The skill does not add application event handlers, API calls, or a production frontend framework.

## Example Request

```text
Use create-wireframe to sketch a customer list for support staff.

Create one screen with a sidebar, a search field, a customer table, and an
Add Customer action. Include empty and loading states. Use the clean style.

Save the source to wireframes/customers.md and render the HTML alongside it.
```

## Review Before Sharing

Confirm that the requested content and states are present, the rendering command succeeds, and the generated HTML is available. When a browser is used, check layout, labels, navigation, and overflow. Report any visual checks that were not performed instead of claiming that the wireframe was reviewed.
