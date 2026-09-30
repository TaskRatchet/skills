# TaskRatchet Skills

Claude Agent Skills for [TaskRatchet](https://taskratchet.com) — a todo list that charges real money when a deadline is missed.

## Skills

- [`taskratchet-tasks`](skills/taskratchet-tasks/SKILL.md) — list, create, complete, edit, and "uncle" your TaskRatchet tasks through an agent, with a required confirmation step before any action that changes what you're financially on the hook for.

## Installing

```
npx skills add TaskRatchet/skills --skill taskratchet-tasks
```

The skill prefers TaskRatchet's [MCP server](https://docs.taskratchet.com/agents) (`https://api.taskratchet.com/mcp`) when it's connected and authorized in your agent, and falls back to the REST API only when MCP isn't connected or isn't logged in, using a TaskRatchet API key (**API Token** on your [account settings](https://app.taskratchet.com/settings) page) set as the `TASKRATCHET_API_KEY` environment variable. If MCP is connected but lacks a permission an action needs, it asks you whether to re-authorize or use the API key instead. If neither is set up, it walks you through connecting one.

## Docs

- [TaskRatchet docs](https://docs.taskratchet.com)
- [MCP reference](https://docs.taskratchet.com/mcp)
- [API reference](https://docs.taskratchet.com/api-reference.html)
- [OpenAPI spec](https://api.taskratchet.com/api2/openapi.json)
