# Skills

[![skills.sh](https://skills.sh/b/naikorasu/skills)](https://skills.sh/naikorasu/skills)

This repository contains agent skills that can be installed with `npx skills add`.

## Installation

Choose the skill you want to install. Each command is provided separately.

### Create Infographic Document

```bash
npx skills add https://github.com/naikorasu/skills --skill create-infographic-document
```

Add `-g` for a global installation. See the [usage guide](./docs/create-infographic-document.md#installation-and-requirements) for requirements.

### Create Wireframe

```bash
npx skills add https://github.com/naikorasu/skills --skill create-wireframe
```

Add `-g` for a global installation. Rendering requires Node.js, npm, and the `wiremd` CLI. See the [usage guide](./docs/create-wireframe.md#installation-and-requirements) for setup instructions.

### Create SEO Article

```bash
npx skills add https://github.com/naikorasu/skills --skill create-seo-article -g
```

This installs the orchestrating skill globally, but does not install its six required skills. Install those dependencies globally as described in the [usage guide](./docs/create-seo-article.md#required-dependencies).

## Skills

| Skill | Documentation |
|-------|---------------|
| [create-infographic-document](./skills/create-infographic-document/SKILL.md) | [Usage guide](./docs/create-infographic-document.md) |
| [create-wireframe](./skills/create-wireframe/SKILL.md) | [Usage guide](./docs/create-wireframe.md) |
| [create-seo-article](./skills/create-seo-article/SKILL.md) | [Usage guide](./docs/create-seo-article.md) |
