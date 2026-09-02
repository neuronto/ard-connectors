# Google Gemini — Agent Finder via remote MCP

MCP works in **Gemini CLI** and the **Gemini API**, not the consumer Gemini app.

> This points at Neuronto, which federates every other public ARD registry. To use a different one, replace the URL with any `mcp` endpoint from `agent-finders.json`.

## Gemini CLI

Add the server to your Gemini CLI `settings.json` (see `settings.json` here):

```json
{
  "mcpServers": {
    "agent-finder": {
      "httpUrl": "https://neuronto.com/mcp"
    }
  }
}
```

Use `httpUrl` for a Streamable-HTTP server, `url` for SSE, or `command`/`args`
for a local/stdio server. Restart the CLI; the Agent Finder search tool is then
available. Pair with the Gem instructions in `../../skills/gemini/` so it asks
first and never auto-installs.

## Gemini API

The Python and JavaScript SDKs can connect to remote MCP servers directly, or you
can expose `POST /search` as a function/tool. See the Gemini API MCP docs.

## Consumer Gemini app

The Gemini app does not support custom MCP servers yet. Use the Gem
(`../../skills/gemini/`) for the behavior, but run actual searches via the CLI or
API above.
