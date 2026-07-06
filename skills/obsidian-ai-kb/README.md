# obsidian-ai-kb

A [Codex](https://github.com/openai/codex) skill that organizes AI-related URLs (GitHub repos, articles, tweets, papers, tutorials, product reviews) into a structured **Obsidian** knowledge base.

## What it does

Give it one or more AI-related URLs and it will:

- Fetch the source content (GitHub repo metadata + README, article HTML, X/Twitter post via fxtwitter, paper PDFs, etc.)
- Route the content to the right section by content type
- Auto-create new top-level sections or subcategories when content does not fit any existing one
- Generate a note using a fixed frontmatter template (project / opinion / tweet / tutorial / paper / review)
- Update the relevant MOC (Map of Content) index
- Archive the original source into `attachments/`

## Knowledge base layout

The skill targets an Obsidian vault rooted at `D:\data\AI`:

```
D:\data\AI\
+-- 00-首页\AI知识库首页.md   # Top-level navigation MOC
+-- 01-提示词\                # Prompt templates
+-- 02-AI项目大全\            # Open-source projects (with subcategories + MOC)
+-- 03-行业观点与洞察\        # Opinions / roundtables / tweets
+-- 04-学习与教程\            # Tutorials & courses
+-- 05-论文与研报\            # Papers & research reports
+-- 06-产品体验\              # Product reviews
+-- attachments\              # Archived source files
```

Sections grow dynamically — empty sections are never pre-created.

## Install

From the parent repo ([odtu945/powercat](https://github.com/odtu945/powercat)), install this skill with the bundled [skill-installer](https://github.com/openai/skills) helper:

```bash
scripts/install-skill-from-github.py --repo odtu945/powercat --path skills/obsidian-ai-kb
# or by URL:
scripts/install-skill-from-github.py --url https://github.com/odtu945/powercat/tree/main/skills/obsidian-ai-kb
```

Or manually clone into your Codex skills directory:

```bash
git clone https://github.com/odtu945/powercat.git
cp -r powercat/skills/obsidian-ai-kb ~/.codex/skills/obsidian-ai-kb
```

After installing, **restart Codex** to pick up the new skill, then invoke it with, for example:

```
Use $obsidian-ai-kb to organize these AI URLs into my Obsidian knowledge base.
```

## Contents

- `SKILL.md` — Main skill instructions (entry point)
- `agents/openai.yaml` — Interface metadata (display name, brand color, default prompt)
- `references/templates.md` — Note frontmatter templates for each content type

## Requirements

- [Codex](https://github.com/openai/codex) CLI / desktop app
- A local Obsidian vault
- The GitHub MCP server (for repo metadata) and network access for fetching URLs

## License

[MIT](../../LICENSE) — Copyright (c) 2026 William Wang
