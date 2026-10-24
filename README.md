# Nakata skills

Agent skills for [Nakata](https://nakata.app), the provider-agnostic agentic CLI.

```bash
npx skills add nakata-app/skills
```

| Skill | Use it when |
|---|---|
| `nakata-cli` | You need to install Nakata, pick a model key, run it interactively or continue a session. |
| `nakata-headless` | You are scripting Nakata from CI, another agent or a shell: one-shot prompts, JSON / NDJSON output, schemas, turn and time caps, permission modes. |

Skills follow the open `SKILL.md` format and install into Claude Code, Codex, Cursor, OpenCode and the other agents the `skills` CLI supports.
