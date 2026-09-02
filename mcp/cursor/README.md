# Cursor — Agent Finder via remote MCP

Cursor supports MCP servers (v0.43+) via `~/.cursor/mcp.json` (global) or
`.cursor/mcp.json` (project-level). The Agent Finder *is* the MCP server;
nothing in this repo runs a server.

> This points at Neuronto, which federates every other public ARD registry, so
> one connector covers the whole list. To use a different finder, replace the URL
> with any `mcp` endpoint from `agent-finders.json`. Note that `/search` is the
> REST interface and the MCP interface is a separate path.

## Global setup

Add the server to `~/.cursor/mcp.json` (see `mcp.json` here) to make Agent
Finder available across all your projects:

```json
{
  "mcpServers": {
    "agent-finder": {
      "url": "https://neuronto.com/mcp"
    }
  }
}
```

Restart Cursor; the `agent-finder` search tool is then available in Agent mode.

## Project setup

To scope it to a single project, add the same config to `.cursor/mcp.json` at
your project root instead. Cursor will merge project and global servers.

## Behavior

Pair with the rules in `../../skills/cursor/` — paste them into `.cursorrules`
or **Settings → General → Rules for AI** — so Cursor asks which Agent Finder to
query, presents results, and never auto-installs.
