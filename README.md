# X Search via Hermes

Claude Code and Codex plugin for searching X/Twitter posts, latest news,
social reactions, account posts, and public discussion through Hermes Agent's
`x_search` toolset and xAI/Grok OAuth.

Version: `1.0.0`

Keywords: `x-search`, `twitter-search`, `hermes-agent`, `xai`, `grok`,
`agent-skill`, `claude-code`, `codex-plugin`, `social-search`, `ai-research`

## Repository Layout

The canonical skill lives inside one shared plugin package:

```text
plugins/x-search-via-hermes/
  .claude-plugin/plugin.json
  .codex-plugin/plugin.json
  skills/x-search-via-hermes/SKILL.md
```

Claude Code and Codex marketplace files both point at that same plugin root, so
there is only one `SKILL.md` to maintain.

## Install in Claude Code

Register this repository as a Claude Code plugin marketplace:

```text
/plugin marketplace add arthurkatcher/x-search-via-hermes
```

Then install the skill plugin:

```text
/plugin install x-search-via-hermes@x-search-via-hermes-skills
```

From a shell, the equivalent commands are:

```bash
claude plugin marketplace add arthurkatcher/x-search-via-hermes
claude plugin install x-search-via-hermes@x-search-via-hermes-skills
```

If the skill does not appear immediately, restart Claude Code.

## Install in Codex as a Plugin

Register this repository as a Codex plugin marketplace:

```bash
codex plugin marketplace add arthurkatcher/x-search-via-hermes --ref main
```

Then install the plugin:

```bash
codex plugin add x-search-via-hermes@x-search-via-hermes-skills
```

Restart Codex after installing.

## Install in Codex as a Direct Skill

Install the skill folder from GitHub:

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo arthurkatcher/x-search-via-hermes \
  --path plugins/x-search-via-hermes/skills/x-search-via-hermes
```

Restart Codex after installing.

## Manual Install

Copy the whole skill folder, not only `SKILL.md`, into your agent's skills
directory:

```bash
mkdir -p ~/.claude/skills
cp -R plugins/x-search-via-hermes/skills/x-search-via-hermes ~/.claude/skills/
```

For Codex:

```bash
mkdir -p ~/.codex/skills
cp -R plugins/x-search-via-hermes/skills/x-search-via-hermes ~/.codex/skills/
```

For generic Agent Skills harnesses:

```bash
mkdir -p ~/.agents/skills
cp -R plugins/x-search-via-hermes/skills/x-search-via-hermes ~/.agents/skills/
```

## Requirements

- Hermes Agent available as `hermes`
- Hermes authenticated with xAI/Grok OAuth
- Hermes `x_search` toolset enabled

The skill contains branches for installing Hermes, launching xAI auth, verifying
`x_search`, and running focused X search prompts.

## License

MIT
