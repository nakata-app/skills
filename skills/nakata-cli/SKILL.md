---
name: nakata-cli
description: Install and drive Nakata, the provider-agnostic agentic CLI (Director + Workers) that reads code, runs tools and ships changes. Use when the user wants to set up Nakata, choose a model key, run it interactively, continue a session, or activate a license.
---

# Nakata CLI

Nakata is an agentic CLI. One Director holds the objective, permissions and final judgment; bounded Workers search, verify, test, review and trace in parallel and return evidence. You bring your own model key; Nakata does not proxy it.

## Install

```bash
npm i -g nakata-cli          # macOS, Linux, Windows 64-bit
nakata --version             # prints "nakata <version> (build <rev> · <time>)"
```

If `svelka-cli` (the old package name) is installed, remove it first: `npm uninstall -g svelka-cli`. Both provide the `nakata` command.

## Model key

Export one provider key in the shell before starting. Any OpenAI-compatible endpoint works.

```bash
export OPENROUTER_API_KEY=...   # or DEEPSEEK_API_KEY, OPENAI_API_KEY, ANTHROPIC_API_KEY, ...
```

Override the model for one run with `--model <id>`. Use `--profile <name>` for an isolated configuration.

## Run

```bash
nakata                       # interactive TUI in the current project
nakata -c                    # continue the most recent session in this directory
nakata -r <session-id>       # continue a specific session
nakata --lean                # core tools only, MCP servers deferred (less prompt bloat)
```

Inside the TUI: `/help` lists commands, `/pricing` shows plans, `/license` shows status, `/activate` opens the browser to activate a paid plan, `/license <token>` pastes an email token. Project commands live in `.svelka/commands/*.md`; MCP servers from `.mcp.json` expose prompts as `/mcp__<server>__<prompt>`.

## Preferences

Tell Nakata style rules in plain English or Turkish ("Keep answers short"). It stores a known-safe set of response and verification rules locally in `~/.nakata/corrections/`.

## Pricing and terms

Plans and the free period are on https://nakata.app/pricing. Terms: https://nakata.app/terms. Support: support@nakata.app.

## Troubleshooting

- `nakata` not found after install: open a new shell or check `npm bin -g` is on PATH.
- "no API key": the key must be exported in the shell that starts Nakata (GUI launchers do not inherit it).
- Permission prompts in automation: see the `nakata-headless` skill for `--permission-mode` and allow/deny rules.
