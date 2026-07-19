---
name: x-search-via-hermes
description: Use when the user wants to search X/Twitter posts, latest news on X, public discussion, account posts, social reactions, or X-sourced data through Hermes. Checks whether Hermes is installed and can use either built-in xAI/Grok x_search or the optional Hermes Tweet plugin before running Hermes search prompts.
---

# X Search via Hermes

Use this skill for read-only X/Twitter research through Hermes. The user-facing goal is X-sourced information; Hermes plus either xAI auth or Hermes Tweet are dependencies.

Do not use this skill for posting, liking, following, DMs, deletion, or other X account mutations. Use an account-control skill/tool for those workflows.

## Fast Path

For any X search request, check only what is needed:

```bash
which hermes
hermes status --all
hermes tools list
```

Proceed directly to search through built-in Hermes X search when:

- `hermes` exists.
- `hermes status --all` shows xAI/Grok OAuth logged in or active.
- `hermes tools list` shows `x_search` enabled.

`x_search` is a Hermes built-in toolset, not necessarily an MCP server. Do not treat `hermes mcp list` showing no MCP servers as a failure.

If built-in X search is not ready, check the optional plugin path:

```bash
hermes plugins list
```

Proceed through Hermes Tweet when:

- `hermes plugins list` shows `hermes-tweet` enabled.
- `hermes tools list` shows the `hermes-tweet` toolset.
- `XQUIK_API_KEY` is configured for the Hermes process when the task needs live reads.

Hermes Tweet exposes `tweet_explore` without an API key. That proves the plugin is installed, but live X reads require `tweet_read` and `XQUIK_API_KEY`.

Prefer built-in `x_search` when it is already enabled and authenticated. Use Hermes Tweet when xAI auth is missing, xAI returns an entitlement error, or the user has configured the Hermes Tweet plugin for public X reads.

Xquik is an independent third-party service. Not affiliated with X Corp. "Twitter" and "X" are trademarks of X Corp.

## If Hermes Is Missing

If `which hermes` fails, install Hermes if the harness can run shell commands with user approval:

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

If the installer enters an interactive setup wizard:

- If the harness can control an interactive terminal, select xAI/Grok OAuth during setup.
- If the harness cannot control an interactive terminal, ask the user to finish setup in their terminal.
- If Hermes prints an xAI auth URL, show that URL to the user clearly and ask them to complete browser login.
- Do not ask the user to paste tokens or inspect Hermes auth files.

After install/setup, rerun:

```bash
which hermes
hermes status --all
hermes tools list
```

## If xAI Auth Is Missing

If Hermes is installed but xAI/Grok OAuth is not logged in, run or ask the user to run:

```bash
hermes auth add xai-oauth
```

If the session is remote or cannot open a browser:

```bash
hermes auth add xai-oauth --no-browser
```

If Hermes prints an auth URL, repeat it to the user. The user must complete the browser login. Do not read or print `~/.hermes/auth.json`.

Then verify:

```bash
hermes status --all
```

## If Provider or Model Is Unset

If xAI OAuth is logged in but Hermes has no usable active provider/model, prefer direct config:

```bash
hermes config set model.provider xai-oauth
hermes config set model.default grok-4.3
```

If direct config fails or Hermes has changed its config keys, ask the user to run:

```bash
hermes model
```

They should choose xAI/Grok OAuth and a Grok model.

## If X Search Is Disabled

Check:

```bash
hermes tools list
```

Expected output includes:

```text
enabled  x_search  X (Twitter) Search
```

If disabled, run:

```bash
hermes tools enable x_search
```

If that fails or requires interaction, ask the user to run:

```bash
hermes tools
```

or:

```bash
hermes setup tools
```

Then select or enable X search.

## If Hermes Tweet Is Available

Use Hermes Tweet only for read-only X research in this skill. Do not call `tweet_action`.

Check:

```bash
hermes plugins list
hermes tools list
```

If Hermes Tweet is missing and the harness can install plugins, run:

```bash
hermes plugins install Xquik-dev/hermes-tweet --enable
```

If Hermes Tweet is installed but disabled, run:

```bash
hermes plugins enable hermes-tweet
```

If `XQUIK_API_KEY` is missing, ask the user to configure it in the Hermes runtime environment. Do not ask them to paste the value into chat. Keep actions disabled:

```bash
export HERMES_TWEET_ENABLE_ACTIONS="false"
```

After configuration, reload or restart the Hermes process, then rerun:

```bash
hermes plugins list
hermes tools list
```

## Search Prompts

Once the chosen search path is ready, do not run an extra ping. Search the user's topic directly.

General latest-news search:

```bash
hermes -z "Use X search to find the latest posts and news about TOPIC. Summarize confirmed facts, social reactions, rumors or commentary, and include X links."
```

Account-specific search:

```bash
hermes -z "Use X search to find recent posts by @HANDLE about TOPIC. Return direct X links and a concise summary."
```

Rumor-sensitive search:

```bash
hermes -z "Use X search for TOPIC. Separate confirmed announcements from speculation. Include dates and source links."
```

Operational/research search:

```bash
hermes -z "Use X search for practical posts about TOPIC. Return concise findings, dates, and source links when available."
```

Hermes Tweet public search:

```bash
hermes -z "Use tweet_explore to find the tweet search endpoint, then use tweet_read to search TOPIC. Do not call tweet_action. Return concise findings, dates, and X links when available." --toolsets hermes-tweet
```

Hermes Tweet account-specific search:

```bash
hermes -z "Use tweet_explore to find user lookup and user tweets endpoints, then use tweet_read to find recent posts by @HANDLE about TOPIC. Do not call tweet_action. Return direct X links and a concise summary." --toolsets hermes-tweet
```

## Reporting

When answering the user:

- Say that Hermes searched X.
- If Hermes Tweet was used, say that Hermes searched X through the Hermes Tweet plugin.
- Include direct X links when Hermes provides them.
- Include dates when available.
- Distinguish confirmed information from reactions, rumors, or commentary.
- If X results are sparse or source links are missing, say so plainly.

## Failure Branches

- Network or DNS failure: rerun with network approval or ask the user to run Hermes in a normal terminal.
- Read-only `~/.hermes` or auth lock errors: ask the user to run the Hermes auth/setup command in a normal terminal.
- xAI `401` or auth failure: rerun `hermes auth add xai-oauth`.
- xAI `403`: likely subscription or entitlement issue; use Hermes Tweet if configured, or ask the user to check Grok/xAI access.
- `x_search` missing: run `hermes update`, then recheck `hermes tools list`.
- Hermes Tweet installed but only `tweet_explore` works: configure `XQUIK_API_KEY` in the Hermes runtime environment, then reload or restart Hermes.
- `tweet_action` unavailable: expected for this skill. Keep `HERMES_TWEET_ENABLE_ACTIONS` unset or `false`.
