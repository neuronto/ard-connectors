# Neuronto Agent Finder for GitHub Copilot

Finds the MCP servers, skills, agents and APIs that can do a task. It searches
the Neuronto ARD Registry, which also covers the other public Agentic Resource
Discovery (ARD) registries, and returns ranked matches with the endpoint to
connect to.

The plugin bundles two remote MCP servers:

| Server | Endpoint | Purpose |
|---|---|---|
| `neuronto-ard-registry` | `https://neuronto.com/mcp` | search for servers, skills, agents, APIs and individual tools |
| `neuronto-payments` | `https://pay.neuronto.com/mcp` | x402 facilitator on Base: put an API behind x402 and buy prepaid credits |

It also includes one skill, `neuronto-agent-finder`, that tells Copilot how to
present results. Nothing is installed automatically.

## Layout

```
copilot-plugin/
  plugin.json
  mcp.json
  skills/neuronto-agent-finder/SKILL.md
  LICENSE
```

When submitting or installing from this repository, the plugin path is
`copilot-plugin`.

## Licence

Apache-2.0. See `LICENSE`.
