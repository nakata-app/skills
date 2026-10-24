---
name: nakata-headless
description: Script Nakata from CI, shells or other agents - one-shot prompts, JSON or NDJSON output, JSON Schema constrained answers, turn and time caps, permission modes and tool allow/deny rules. Use when Nakata must run without a human at the keyboard.
---

# Nakata headless

`nakata -p` runs one prompt and exits. Everything below is from `nakata --help`.

## One-shot

```bash
nakata -p "explain this project"
echo "list failing tests" | nakata            # prompt from stdin
nakata -c -p "now fix the first one"          # continue the latest session in this dir
```

## Output formats

```bash
nakata -p "..." --output-format text          # human stream (default)
nakata -p "..." --output-format json          # one JSON result object
nakata -p "..." --output-format stream-json   # one NDJSON event per engine event
nakata -p "..." --json-schema '{"type":"object","properties":{"ok":{"type":"boolean"}},"required":["ok"]}'
                                              # final output constrained to the schema (implies json)
```

With `--input-format stream-json` stdin stays open for JSON user messages; add `--ask-stdin` to receive `{"type":"ask_user"}` lines and answer with `{"type":"ask_answer"}` instead of auto-picking the first option.

## Guards

```bash
--max-turns <n>          # cap the agentic loop by turns (runaway / cost guard)
--max-time <seconds>     # cap by wall clock (or NAKATA_MAX_TIME)
--model <id>             # pin the model for this run
--lean                   # core tools only; MCP servers deferred until requested
--best-of-n <N>          # run N ways in isolated git worktrees, keep the best (N=2..5)
```

## Permissions

```bash
--permission-mode default|acceptEdits|plan|bypassPermissions|dontAsk
--allowedTools "Read,Bash(git status:*)"      # allow rules
--disallowedTools "Bash(rm:*)"                # deny rules win over any mode
--auto                                        # auto-approve SAFE tools only; risky tools still ask
--dangerously-skip-permissions                # alias for bypassPermissions (deny rules still win)
```

For unattended CI prefer `--permission-mode acceptEdits` or `--auto` plus explicit `--disallowedTools`, and always set `--max-turns` and `--max-time`.

## Recovery

`--auto-resume` checkpoints after a guard or error stop and schedules `--continue`; `--stop-auto-resume <sessionId>` removes that job.

## Exit and parsing

Parse `--output-format json` for the result; use `stream-json` when you need progress events. Keep the model key in the environment of the process that launches Nakata.
