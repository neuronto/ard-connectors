# Cursor — Agent Finder via remote MCP

Cursor supports MCP servers (v0.43+) via `~/.cursor/mcp.json` (global) or
`.cursor/mcp.json` (project-level). The Agent Finder *is* the MCP server;
nothing in this repo runs a server.

> Replace `https://agent-finder.example.org/mcp` with your Agent Finder's MCP
> endpoint. The `/search` path is the REST interface; the MCP interface is
> usually a separate path (e.g. `/mcp`). Use whatever URL your Agent Finder
> publishes.

## Global setup

Add the server to `~/.cursor/mcp.json` (see `mcp.json` here) to make Agent
Finder available across all your projects:

```json
{
  "mcpServers": {
    "agent-finder": {
      "url": "https://agent-finder.example.org/mcp"
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
