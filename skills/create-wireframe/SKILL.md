---
name: create-wireframe
description: Create, render, and iterate on UI wireframes from descriptions, requirements, or existing interfaces using wiremd Markdown. Use when a user asks for a wireframe, mockup, screen sketch, clickable prototype, or preview of a wiremd .md file.
compatibility: Requires Node.js and npm for installing @eclectic-ai/wiremd; rendering requires the wiremd CLI. A browser is optional for visual review.
---

# Create Wireframe

Create a readable `.md` source file and render it to HTML with wiremd. Use the installed `wiremd` CLI and ordinary file and shell tools; this skill does not require Claude Code, a repository checkout, or a particular agent platform.

## Workflow

1. Establish the screen's purpose, audience, key content, and expected output location. Use the current working directory when no destination is specified. If the request does not make clear whether the deliverable is one screen or a connected multi-page flow, ask before creating files.
2. If documenting an existing UI, inspect its visible structure and states. Represent navigation, headings, form labels, actions, data columns, and empty, loading, or error states. Omit event handlers, API calls, and implementation details that do not affect the wireframe.
3. Check CLI availability with `wiremd --version`. If it is missing, install the published package with `npm install -g @eclectic-ai/wiremd`, after obtaining permission for a global installation when necessary. Check `wiremd --version` again. Run `wiremd --help` for the installed CLI's supported flags, styles, and examples; treat it as the local CLI reference when it differs from online documentation. Do not clone a Git repository or assume a specific package version.
4. Write the `.md` file in visual reading order: navigation, main content, forms or data, then supplementary states. Keep labels and meaningful actions faithful to the request. Use `clean` as the skill's default style unless the user chooses another; the CLI's own default is `sketch`.
5. Render the file, check the command's exit status, and inspect the resulting HTML. If browser access is available, open the generated file and correct any broken layout or missing content. Preserve the Markdown source as the editable deliverable.
6. Report the Markdown and HTML paths, chosen style, preview method, and any unverified visual details.

## Rendering

```sh
wiremd screen.md -o screen.html --style clean
```

For live local preview, run `wiremd screen.md --serve --watch --style clean` and open the address printed by the CLI **only if the user's browser can reach that same machine**. `--serve` without `--watch` does not enable live updates for a single file. If the agent runs on a remote or isolated host, provide the generated HTML file instead of claiming its localhost URL works for the user. For a directory of screens, `wiremd ./screens --style clean` renders page files; `wiremd ./screens --serve --style clean` serves the directory and watches its Markdown files. Files beginning with `_` act as shared partials and are not rendered as standalone pages.

Other documented styles are `sketch`, `wireframe`, `material`, `tailwind`, `brutal`, and `none`. Select a different style only when the user's intent warrants it.

## Wiremd syntax

Use standard Markdown for headings, lists, links, tables, and notes. Use wiremd syntax for UI elements:

| Element | Syntax | Constraint |
| --- | --- | --- |
| Primary button | `[Save]*` | A plain `[Save]` is a regular button. |
| Text, email, or password input | `[Name________]`, `[________]{type:email}`, `[********]` | Put the label immediately above the field without a blank line. |
| Select | `[Choose________v]` followed immediately by `- Option` lines | Its following list supplies options. |
| Checkbox or radio | `- [ ] Agree`, `- [x] Agree`, `- ( ) A`, `- (*) B` | Use the marked forms for selected states. |
| Navigation or breadcrumbs | `[[ Brand | *Home* | [About](./about.md) ]]`, `[[ Home > Account ]]` | Use links for destinations and `*...*` for the current item. |
| Badge | `((Active)){success}` | Variants include `warning`, `error`, and `primary`. |
| Card or sidebar | `::: card` or `::: sidebar`, then content, then `:::` | A sidebar before page content creates a sidebar/main layout. |
| Columns or row | `::: columns-3` with `::: column Title` children; `::: row {right}` | Close each nested container with its own `:::`. |
| Linked action | `[[View details](./detail.md)]*` | Use for clickable multi-page prototypes. |
| Shared partial | `![[_nav.md]]` | Include paths resolve relative to the including file. |

Place a blank line before a closing `:::` when its final child is a list or an inline button, link, badge, or other inline syntax. Avoid a blank line between an input label and its field. Consult the [syntax reference](https://tobiashoelzer.com/wiremd/reference/syntax.html) for more components and attributes; do not assume an element has a dedicated visual renderer merely because its syntax parses.

### Minimal example

Save this example as `dashboard.md` and render it with the command above, replacing `screen.md` with `dashboard.md`:

```wiremd
[[ Acme | *Dashboard* | Settings ]]

::: sidebar
[Overview]*
[Reports]
:::

## Dashboard

::: columns-2 card
::: column Active users
**1,240**
:::
::: column Revenue
**$12,400**
:::
:::

## Sign in

Email
[_____________________]{type:email required}

Password
[*********************]{type:password required}

[Sign In]* [Forgot password?]
```

For multi-page work, create one `.md` file per page and include shared navigation with `![[_nav.md]]`. Use documented `[[Label](./page.md)]` links for navigation and prefer the directory dev server for clickable previews. Static HTML output can retain `.md` link targets; verify or adapt those links before promising a standalone, locally clickable HTML bundle.

## References

- [Wiremd overview](https://tobiashoelzer.com/wiremd/guide/overview.html)
- [CLI installation and usage](https://tobiashoelzer.com/wiremd/guide/installation.html)
- Local CLI help: run `wiremd --help` for available options and usage examples.
- [CLI options](https://tobiashoelzer.com/wiremd/reference/cli.html)
- [Components and syntax](https://tobiashoelzer.com/wiremd/components/)
- [Shared components and multi-page prototypes](https://tobiashoelzer.com/wiremd/components/includes.html)
