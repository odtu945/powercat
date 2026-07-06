# powercat

A collection of [Codex](https://github.com/openai/codex) skills.

## Skills

| Skill | Description |
| --- | --- |
| [obsidian-ai-kb](skills/obsidian-ai-kb) | Organize AI-related URLs (GitHub repos, articles, tweets, papers, tutorials, product reviews) into a structured Obsidian knowledge base. |

## Repository layout

```
powercat/
+-- LICENSE                 # MIT (covers the whole repo)
+-- README.md               # This file (skills index)
+-- skills/
    +-- <skill-name>/       # One folder per skill
        +-- SKILL.md        # Skill entry point
        +-- ...             # agents/, references/, etc.
```

Each skill lives in its own subfolder under `skills/`, so you can install them individually by path.

## Install a skill

Use the bundled [skill-installer](https://github.com/openai/skills) helper to install a skill into `$CODEX_HOME/skills` (defaults to `~/.codex/skills`):

```bash
# Install one skill by repo + path
scripts/install-skill-from-github.py --repo odtu945/powercat --path skills/obsidian-ai-kb

# Or by URL
scripts/install-skill-from-github.py --url https://github.com/odtu945/powercat/tree/main/skills/obsidian-ai-kb
```

Options: `--ref <ref>` (default `main`), `--dest <path>`, `--method auto|download|git`. It aborts if the destination skill directory already exists. After installing, **restart Codex** to pick up the new skill.

### Manual

```bash
git clone https://github.com/odtu945/powercat.git
# then copy the skill folder you want into your skills dir:
cp -r powercat/skills/obsidian-ai-kb ~/.codex/skills/
```

## License

[MIT](LICENSE) — Copyright (c) 2026 William Wang
