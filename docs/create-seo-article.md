# Create SEO Article

## Purpose

The `create-seo-article` skill coordinates globally installed SEO and writing skills to produce one final article in a `.md` file. It begins with a keyword or topic, gathers missing context interactively, researches search intent, writes the draft, and applies two sequential editing passes.

The final file contains only the article: its title, introduction, body, conclusion, relevant citations, and agreed CTA. It does not contain the content brief, competitor analysis, SEO report, metadata block, alternative copy, or UI code.

The [skill instructions](../skills/create-seo-article/SKILL.md) contain the complete execution rules and dependency checks.

## Installation

Install the orchestrating skill globally with:

```sh
npx skills add https://github.com/naikorasu/skills --skill create-seo-article -g
```

This command does not install the skill's dependencies. The agent checks their actual global installations before starting any article work.

## Required Dependencies

All six skills below must be globally installed, readable, and nonempty, even when the initial request already contains every article input.

| Skill | Responsibility | Dependency type |
|-------|----------------|-----------------|
| `seo-content-brief` | It researches SERPs, competitors, search intent, content gaps, and the outline. | It is a direct dependency. |
| `write-content` | It writes the complete draft from the brief and verified context. | It is a direct dependency. |
| `article-content` | It improves the long-form body, information gain, structure, and readability. | It is a direct dependency. |
| `copywriting` | It improves the headline, hook, CTA, and evidence-backed persuasion. | It is a direct dependency. |
| `grill-me-interactive` | It gathers missing inputs and confirms decisions through `ask_user`. | It is a direct dependency. |
| `grill-me` | It supplies the interview protocol loaded by the interactive wrapper. | It is a transitive dependency of `grill-me-interactive`. |

Known installation sources for the four content skills are:

```sh
npx skills add https://github.com/agricidaniel/claude-seo --skill seo-content-brief -g
npx skills add https://github.com/inhouseseo/superseo-skills --skill write-content -g
npx skills add https://github.com/kostja94/marketing-skills --skill article-content -g
npx skills add https://github.com/kostja94/marketing-skills --skill copywriting -g
```

Install the approved `grill-me` and `grill-me-interactive` skill directories together in a global skill location. The wrapper's relative `../grill-me/SKILL.md` reference must resolve. If their installation source is unknown, supply an approved source rather than using a guessed repository.

The host must also provide `ask_user` and file-reading and file-writing tools. Web research requires search and page-fetching access or sufficient verifiable SERP and competitor material supplied by the user. DataForSEO and Ahrefs integrations are optional.

The installed content skills must retain their required supporting references. The `eeat-audit` template dependency is conditional: it is needed when `write-content` selects an external `thought-leadership` or `product-reviews` template. The workflow stops if that route requires an unavailable template. The [dependency section](../skills/create-seo-article/SKILL.md#required-skill-dependencies) identifies the relevant reference files and installation checks.

UI and page-generator skills are not dependencies of this workflow.

## Missing-Dependency Behavior

If any required dependency is missing, unreadable, empty, or cannot be verified as globally installed, the agent stops before interviewing, researching, drafting, editing, or writing the article. It reports the unresolved dependencies and recommends installation directly.

The agent does not install automatically, substitute another skill, or generate a partial article. After the user completes installation, the entire preflight must pass again before work resumes.

## Article Inputs

Begin with a keyword or topic and include any context already known:

| Input | Description |
|-------|-------------|
| Keyword or topic. | Identify the primary subject and any secondary terms. |
| Audience and market. | Identify the reader, knowledge level, region, and article language. |
| Business or editorial context. | Identify the publisher, relevant offer, and website when applicable, or state that the article is independent editorial content. |
| Goal and intent. | Explain what the reader should accomplish. The workflow verifies search intent against research rather than guessing it. |
| Voice and constraints. | Specify tone, brand voice, prohibited subjects, and compliance requirements. |
| Sources and unique knowledge. | Provide competitor material, reliable references, expert input, or first-party examples when available. |
| CTA. | Specify the intended reader action and a real destination when a link is needed. |
| Output path. | Specify the exact `.md` file that should contain the final article. |

Missing decisions are resolved through `grill-me-interactive`, one focused `ask_user` question at a time. The agent reuses facts already supplied or available in approved context instead of asking again.

If no filename is supplied, the agent asks for it and offers a suggested path for approval. It does not silently default to the current directory. When a filename is supplied, it preserves that path and requests permission before overwriting existing content. It also confirms shared understanding before producing the article when that confirmation has not already been given.

Business-context memory files, research files, and draft files are not created unless separately requested or approved.

## Content Workflow

```text
Keyword / Topic + agreed article context
→ seo-content-brief
→ Verified SERP / Competitor Analysis / Search Intent + Content Brief
→ write-content
→ Human-like SEO Article
→ article-content
→ Improved Article Body
→ copywriting
→ Improved Hook + CTA + Persuasion
→ Final Article (.md)
```

1. `seo-content-brief` builds the research-backed brief, verifies intent, and identifies a specific contribution beyond existing articles.
2. `write-content` uses that brief and approved context to write the complete article without repeating research already performed.
3. `article-content` improves the body, evidence, structure, transitions, and readability while preserving useful depth.
4. `copywriting` refines the headline, opening hook, benefit framing, and CTA without turning the article into a sales page.
5. The agent validates the article, saves it to the confirmed path, and reads the file back to check that it is complete and contains only the intended article.

Both refinement skills run sequentially. Briefs, source notes, and draft handoffs remain working context rather than additional final deliverables.

## Quality Rules

The agreed brief and search intent govern structure and SEO choices. Keywords must be natural; the workflow does not impose a fixed keyword-density target, a quota of keyword-bearing H2s, or padding to satisfy a word count.

Specific numbers, quotations, examples, and experience claims must be supported by real sources or user-provided evidence. If the user has no first-party experience to contribute, the article can use agreed, verified source synthesis without pretending that the author performed tests or had experiences.

Human-like writing is a style objective, not a guarantee of passing AI detectors. Human fact-checking and editorial review remain necessary before publication.

## Example Request

```text
Use create-seo-article to write about "cara memilih mesin espresso untuk kedai kecil".

Audience: first-time coffee shop owners in Indonesia.
Language: Indonesian.
Context: an independent editorial guide, not a product promotion.
Tone: practical and conversational.
CTA: ask readers to list their shop's requirements before buying.
Do not invent prices, statistics, product claims, or personal experiences.

Save only the final article to articles/panduan-mesin-espresso.md.
```

## Completion

The agent reports the saved file path and, only when necessary, a concise factual limitation outside the file. It does not add the intermediate brief, SEO report, framework explanation, or alternate copy to the article or final delivery.
