---
name: create-seo-article
description: Create a final SEO article from a keyword or topic by coordinating globally installed seo-content-brief, write-content, article-content, and copywriting skills. Use when the user wants a complete SEO article saved as Markdown. Check every required skill before starting, stop and recommend installation when any dependency is missing, and use grill-me-interactive for missing information or an unspecified destination.
compatibility: Requires the six globally installed skills listed below, an ask_user tool, and file-reading and file-writing tools. Research requires web access or verifiable SERP and competitor material supplied by the user.
metadata:
  version: "1.0.0"
  required-skills: "seo-content-brief, write-content, article-content, copywriting, grill-me-interactive, grill-me"
  output: "article-content-only-md"
---

# Create SEO Article

Coordinate the installed content-writing skills to produce one final article file. The final deliverable must contain only the article: its title, introduction, body, conclusion, relevant citations, and agreed call to action. Do not include the content brief, competitor analysis, SEO report, metadata block, framework explanation, alternative headlines, CTA options, or process commentary.

This skill does not create interfaces, landing-page templates, HTML, CSS, JavaScript, schema markup, or publishing infrastructure. Do not invoke UI or page-generator skills.

## Required Skill Dependencies

Every skill in this table is mandatory, including the interview skills when the initial request already appears complete. Both `article-content` and `copywriting` are required and run sequentially; they are not alternatives.

| Skill | Dependency type | Responsibility | Installation source |
|-------|-----------------|----------------|---------------------|
| `seo-content-brief` | Direct. | It researches SERPs, competitors, search intent, content gaps, and the article outline. | The known source is `agricidaniel/claude-seo`. |
| `write-content` | Direct. | It writes the complete human-like article from the approved brief and verified context. | The known source is `inhouseseo/superseo-skills`. |
| `article-content` | Direct. | It improves the article body, information gain, answer-first structure, transitions, and readability. | The known source is `kostja94/marketing-skills`. |
| `copywriting` | Direct. | It improves the headline, opening hook, CTA, and evidence-backed persuasion without replacing the long-form body. | The known source is `kostja94/marketing-skills`. |
| `grill-me-interactive` | Direct. | It gathers missing information and confirms decisions through `ask_user`. | Use an approved installation source for this exact interactive wrapper. Do not assume a public repository exists. |
| `grill-me` | Transitive through `grill-me-interactive`. | It provides the interview protocol that the interactive wrapper loads from `../grill-me/SKILL.md`. | Install the approved upstream skill alongside the interactive wrapper so that its relative reference resolves. |

The dependency table and frontmatter are declarations, not an automatic dependency installer. Perform the checks yourself on every invocation.

### Required Tools and Supporting Files

- The host must provide `ask_user`, file-reading tools, and file-writing tools. If a required tool is unavailable, stop and recommend enabling it; do not silently replace interactive questions with plain-text questions or claim that a file was saved.
- The installed skill directories must include the references needed for the chosen workflow. For `seo-content-brief`, check `references/excluded-domains.md`, `references/keyword-density.md`, and `references/page-type-templates.md`. For `write-content`, check `references/content-types-overview.md` and the selected article template under `references/content-types/`.
- A conditional template dependency is `eeat-audit` when `write-content` selects an external `thought-leadership` or `product-reviews` article template. Check the referenced template under the installed `eeat-audit/references/content-types/` directory before using that route. If it is unavailable, stop and recommend installing that dependency from the approved `inhouseseo/superseo-skills` source, or ask whether the user wants a supported article type. Never substitute a different type without agreement. Pricing pages and about pages are outside this article-only workflow.
- Web search and page-fetching capabilities are needed unless the user supplies sufficient verifiable SERP and competitor material. DataForSEO and Ahrefs integrations are optional, not required dependencies.
- Skills mentioned only as related reading by the dependencies are not automatically required. Do not load `article-page-generator`, UI skills, or other unrelated skills. An expert-interview skill is unnecessary because the required interview wrapper gathers first-party input.

## 1. Perform the Global Installation Preflight

Complete this gate before interviewing, researching, drafting, editing, or creating article files.

1. Inspect the host's available-skill registry and configured global skill locations. Common locations include `~/.agents/skills/`, `~/.pi/agent/skills/`, and `~/.claude/skills/` when used by the host. Resolve actual paths and symlinks instead of hardcoding a user's home directory.
2. Confirm that all six required names have globally installed, readable, nonempty `SKILL.md` files with matching frontmatter names. Match the declared skill name, not only the directory name; an upstream directory can have a different name. A project-local copy, a catalog entry, or a name mentioned in conversation does not prove global installation.
3. Read each required `SKILL.md` in full. Use its installed instructions rather than reproducing them from memory. Resolve supporting files relative to that skill's actual directory, and confirm that the interactive wrapper's `../grill-me/SKILL.md` exists. Do not preload unrelated reference files.
4. Verify required tools and currently applicable supporting files. Recheck any conditional dependency as soon as a later decision makes it necessary, before executing the dependent step.
5. If any required skill is missing, unreadable, empty, or cannot be verified as globally installed, stop immediately. Report every unresolved dependency and give a direct installation recommendation. Do not interview, run any content-writing skill, generate a partial article, bypass the gate, install automatically, or substitute another skill.

Use a verified source from the table when recommending installation. For example, recommend only the commands relevant to missing dependencies:

```sh
npx skills add https://github.com/agricidaniel/claude-seo --skill seo-content-brief -g
npx skills add https://github.com/inhouseseo/superseo-skills --skill write-content -g
npx skills add https://github.com/kostja94/marketing-skills --skill article-content -g
npx skills add https://github.com/kostja94/marketing-skills --skill copywriting -g
```

For either grill skill, recommend installing the approved `grill-me` and `grill-me-interactive` directories together in a global skill location. If their source is unknown, include a request for an approved source in the blocked-status recommendation rather than inventing a repository or entering the article interview. Resume only after installation is complete and the entire preflight passes again.

## 2. Complete the Article Inputs Interactively

Start with the user's original request. Reuse facts already supplied in the conversation, approved business context, or relevant project material. Read an existing `contextus.md` when applicable. Do not ask for information that can be verified from the environment, and do not reopen settled decisions without new evidence.

Resolve only missing information that affects this article:

| Input | Information to establish |
|-------|--------------------------|
| Keyword or topic. | Establish the primary subject and any supplied secondary keywords. |
| Audience and market. | Establish the intended reader, knowledge level, target country or region, and article language. |
| Business or editorial context. | Establish the publisher, relevant product or service, site URL when applicable, or an explicitly independent editorial context. |
| Goal and search intent. | Establish the reader's desired outcome. Verify search intent through supplied evidence or SERP research rather than assuming it. |
| Voice and constraints. | Establish the appropriate tone, brand voice, prohibited topics, and relevant compliance requirements. |
| Research and unique value. | Establish available competitor material, sources, expert knowledge, examples, and the specific contribution beyond existing articles. |
| CTA. | Establish the desired action and destination when a link is needed. Use an editorial next step if agreed; do not invent an offer, product, or URL. |
| Output path. | Establish the exact destination for the final `.md` file. |

For every unresolved decision, follow the installed `grill-me-interactive` skill and its upstream `grill-me` protocol. Deliver one focused question per `ask_user` call and wait for the answer before choosing the next question. Include a short factual `context` and a clearly marked recommendation; put the recommended option first among two to five meaningful choices. Set `allowFreeform: true`, `allowMultiple: false` unless independent selections are genuinely needed, and `displayMode: "inline"`. Never batch the interview or repeat a question whose answer is already known.

Every question must include an `options` array of objects with `title` and, when useful, `description`. Put the recommendation inside `context` and the first option; do not invent a separate `recommendation` tool parameter. Adapt the following destination-question example to the actual topic, known context, available paths, and user's language:

```json
{
  "question": "Where should the final Markdown article be saved?",
  "context": "The article context is agreed, but no output file was specified. Recommendation: articles/keyword-slug.md.",
  "options": [
    { "title": "Use the recommended path", "description": "Save the final article to articles/keyword-slug.md." },
    { "title": "Choose another path", "description": "Provide the exact .md file path in your response." },
    { "title": "Pause", "description": "Do not create an article file yet." }
  ],
  "allowFreeform": true,
  "allowMultiple": false,
  "displayMode": "inline"
}
```

Wait for the answer. A proposed filename is not a confirmed destination, and no deliverable is created while a required decision remains open.

If the user supplies an exact output file, preserve that path. If no file is specified, ask for the destination through this interview protocol; propose a keyword-based `.md` filename as a recommendation, not an automatic default. If only a directory is supplied, settle the filename. Resolve a conflicting extension with the user before writing. Check whether the destination already contains a file and obtain explicit overwrite approval when needed.

If research tools cannot access a required source, explain the limitation and ask for usable URLs, excerpts, or a SERP export. Do not fabricate rankings, competitor scores, intent evidence, or page content. If neither research access nor sufficient source material is available, pause the workflow.

Restate the agreed context and obtain confirmation of shared understanding before article production, unless that understanding was already explicitly confirmed. If an answer is cancelled or unclear, pause instead of guessing. Do not create a business-context memory file unless the user separately approves its exact location; the final article is the only default file deliverable.

## 3. Execute the Content Flow in Order

Treat intermediate inputs below as handoffs between skills. They are not additional questions when the required information is already known.

```text
Input: Keyword / Topic + agreed article context
    ↓
Skill: seo-content-brief
    ↓
Handoff: Verified SERP / Competitor Analysis / Search Intent + Content Brief
    ↓
Skill: write-content
    ↓
Handoff: Human-like SEO Article
    ↓
Skill: article-content
    ↓
Handoff: Improved Article Body
    ↓
Skill: copywriting
    ↓
Handoff: Improved Hook + CTA + Persuasion
    ↓
Result: Final Article (.md)
```

### Stage A: Build the Brief with `seo-content-brief`

Supply the keyword, audience, market, editorial or business context, relevant URLs, and available research. Follow the installed skill's research and brief structure. Examine actual competitor pages, classify intent, and identify a specific information-gain opportunity supported by real evidence.

Use site structure and real internal-link targets when a publishing site exists. For independent editorial content or unavailable site structure, state the limitation in the internal brief; never invent a sitemap, service, category, or link target. Route material uncertainty through the interview wrapper.

Keep the brief, competitor analysis, source notes, and metadata recommendations as working context. Do not save or present them as additional final deliverables unless the user separately requests them.

### Stage B: Draft with `write-content`

Pass the complete brief, verified sources, approved context, and available first-party knowledge to `write-content`. Reuse the existing SERP analysis and skip duplicate research. Load the applicable content-type reference and article template from the installed dependency, checking any conditional template dependency before drafting.

Reuse the agreed article type and knowledge-extraction answers. If a material choice or unique-knowledge input remains unresolved, ask through `grill-me-interactive`. When the user has no first-party evidence, obtain agreement to use verified source synthesis without pretending it is personal experience.

Write the complete article in the agreed language. Follow the anti-slop and voice rules without inventing numbers, studies, anecdotes, quotations, credentials, or first-person experiences. Keep the draft as working context rather than a separate file deliverable.

### Stage C: Improve the Body with `article-content`

Review the complete draft against the brief. Improve information gain, answer-first sections, transitions, evidence, readability, and scannability. Remove repetition and filler while preserving the intended depth, factual accuracy, citations, and useful structure.

Do not restart the workflow or convert the article into a page design. Apply summaries or takeaway blocks only when useful for the article's intent. Keep outlines and CTA alternatives outside the final article.

### Stage D: Improve the Hook, CTA, and Persuasion with `copywriting`

Use the improved draft, reader's needs, approved CTA, and real differentiators. Refine the final headline, opening hook, benefit framing, and natural CTA placement. Use persuasion frameworks internally without naming them in the article.

Choose one coherent final version rather than delivering options. Do not replace the long-form article with sales copy, introduce clickbait, exaggerate claims, invent scarcity, or add an unapproved product promise. Preserve the brief's search intent and verified evidence.

## 4. Resolve Conflicting Instructions

Respect host safety rules and explicit user decisions. Within this content workflow, the agreed brief and search intent govern article structure and SEO choices; `write-content` governs natural voice and anti-slop editing; `article-content` governs body refinement; `copywriting` governs short conversion elements.

Use keywords naturally in relevant high-value locations. Do not impose a fixed keyword-density target, force the primary keyword into a quota of H2s, or pad the article to satisfy word-count bands. Brief word counts are planning guidance; depth follows the reader's needs and verified competition. Preserve genuine information gain through both editing passes.

Examples and performance claims in dependency instructions are not evidence for this article. Verify any claim before publication or remove it. Write the article in complete, natural prose rather than compressed conversational shorthand. Human-like writing is a style objective, not a guarantee of passing AI detectors or a license to invent lived experience.

## 5. Validate and Save Only the Final Article

Before writing, confirm that:

- The article fulfills the approved search intent, outline coverage, audience, language, and constraints.
- The hook is relevant, the body contributes supported value, and the CTA matches the agreed reader action.
- Specific claims, quotations, examples, and links are verified and appropriately attributed. No fabricated data or personal experience remains.
- Keyword use is natural, headings are coherent, and no filler, blacklisted phrasing, or repeated sections remain.
- The destination is confirmed, writable, and a `.md` file. Existing content will not be overwritten without permission.
- The deliverable contains one article, not the brief, research tables, metadata, alternate copy, instructions, UI code, or a process report.

Write the final article to the confirmed path using the host's file-writing tools. Read the saved file back and verify that it is nonempty, complete, and contains only the intended article. If writing or verification fails, report the failure and do not claim completion.

Finish with the saved file path and, only when necessary, a concise factual limitation outside the file. Do not dump intermediate work into the final response or create auxiliary files without an explicit request. A saved draft does not remove the need for human fact-checking and editorial review before publication.
