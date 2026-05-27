# X Search via Hermes

Agent Skill for searching X/Twitter posts, latest news, social reactions,
account posts, and X-sourced public discussion through Hermes Agent's
X search capability. It is distributed as a universal `SKILL.md` workflow first,
with Claude Code and Codex plugin manifests for marketplace installation.

**[→ View canonical SKILL.md](plugins/x-search-via-hermes/skills/x-search-via-hermes/SKILL.md)**

> The skill is deliberately placed under `plugins/x-search-via-hermes/skills/x-search-via-hermes/`
> so the same repository can serve as both a Claude Code and Codex plugin marketplace
> from a single canonical `SKILL.md`. This avoids duplication and symlink issues on GitHub.

## What This Skill Allows

This skill helps an agent use Hermes for read-only research on X/Twitter:

- Search current X posts, threads, profiles, and public discussion.
- Track latest news, announcements, reactions, and rumors on X.
- Search recent posts from a specific account or handle.
- Return concise summaries with source links when Hermes provides them.
- Separate confirmed announcements from speculation or commentary.
- Check whether Hermes is installed, authenticated, and ready before searching.

It does not post, like, follow, DM, delete, or otherwise mutate an X account.

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
- Hermes authenticated with a Grok-capable account or API key
- Hermes X search tool enabled

Supported credential paths:

- SuperGrok subscription through xAI OAuth
- X Premium+ subscription through xAI OAuth
- X Premium when xAI enables the account for this OAuth/API surface
- Paid `XAI_API_KEY` as a fallback credential path

Hermes' documented OAuth path is SuperGrok or X Premium+. Access is ultimately
controlled by xAI account entitlements, so a browser login can succeed while
Hermes later receives `403` from the API. In that case, use an eligible
subscription tier or configure a paid `XAI_API_KEY`.

The skill contains branches for installing Hermes, launching xAI auth, verifying
X search availability, and running focused X search prompts.

## Metadata

Version: `1.0.0`

Keywords: `agent-skill`, `x-search`, `twitter-search`, `hermes-agent`, `xai`,
`grok`, `claude-code-plugin`, `codex-plugin`, `social-search`, `ai-research`

## License

MIT
