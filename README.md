# Tstamps agent skill

One agent skill for [Tstamps](https://github.com/havspect/tstamps), team time
tracking. It teaches an agent to read hours, start and stop the running entry,
add or correct entries, produce reports, and derive candidate entries from a
calendar or from past agent work. The agent reaches the workspace through the
`tstamps mcp` server over stdio, or through the `tstamps` CLI when the tools are
not available and it has a shell.

## Prerequisites

- The `tstamps` CLI is installed and on `PATH`.
- `tstamps login --url https://tstamps.example.com` has been run on the machine,
  or you have `TSTAMPS_URL` and `TSTAMPS_TOKEN` to pass to the MCP server.
- For an agent, create a dedicated token with `tstamps tokens create agent`. It
  is shown once.
- A workspace URL, and an account whose permissions cover what you ask the
  agent to read or write.

The MCP server uses the same config file as the CLI
(`~/.config/tstamps/config.json`, mode `0600`). Environment variables
`TSTAMPS_URL` and `TSTAMPS_TOKEN` override it.

## Documentation

Setup for every client — Claude Code and the desktop app, Codex, Langdock and
OpenCode — lives in the Tstamps manual:
[Agents](https://github.com/havspect/tstamps/blob/main/docs/guide/agents/index.md).

## Install

```sh
amp skill add havspect/tstamps-skills/skills/tstamps --global
apm install havspect/tstamps-skills#v1.0.0
gh skill install havspect/tstamps-skills tstamps --agent <agent> --scope user
```

`<agent>` is `claude-code`, `codex`, `opencode` or `amp`. Amp reads the skill's
sibling `mcp.json` and loads the Tstamps tools with the skill. APM and
`gh skill install` copy that file too, but the other clients ignore it, so
configure the MCP server separately as the manual's agent pages show.

## What the skill does

- Reads projects, entries and reports within the token's permissions.
- Proposes a candidate table with project, date, start, end, timezone and note
  before any write, then waits for confirmation. If the user's message already
  states the exact project and times, it echoes them as it logs.
- Never stops a running entry, deletes an entry or moves hours between projects
  unless asked, and says what will happen first.
- Keeps the entry's timezone as the thing that decides the day, and reports the
  date range it actually used.

## What it does not do

- It cannot create or edit users, teams or projects. It reads projects, and
  admins can list teams and users through the MCP server.
- It cannot restore deleted entries or archived projects, change permissions or
  use the admin console.
- It does not approve timesheets, handle leave or produce invoices.

## License

AGPL-3.0-only. See [LICENSE](LICENSE).
