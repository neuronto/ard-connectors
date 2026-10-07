# Neuronto Agent Finder

This extension connects Gemini CLI to two remote MCP servers. Neither needs an
API key to read or search.

## Neuronto ARD Registry (`neuronto-ard-registry`)

A search index of MCP servers, skills, agents and APIs, built on Agentic Resource
Discovery (ARD). One query also covers the other public ARD registries.

Tools:

- `find_resource`: search for a server, skill, agent or API that can do a task.
  Returns ranked matches with a relevance score and the endpoint to connect to.
- `find_tool`: search individual MCP tools by name and behaviour, rather than
  the servers that host them.
- `registry_stats`: how large the index is and how much of it has been verified.
- `publish_resource`: list an MCP server, or a domain that serves an ARD
  manifest, so other agents can discover it. Use only when the user asks to
  publish something.

## Neuronto Payments (`neuronto-payments`)

An x402 facilitator on Base. Tools:

- `facilitator_status` and `settlement_price`: what can be settled right now and
  what it costs.
- `find_paid_resource`: search the catalogue of resources that have been paid
  for through this facilitator.
- `integration_snippet`: working code to start charging for a route.
- `monetize_api`: put an existing HTTPS API behind x402 without changing its
  code.
- `buy_credits`: how to buy prepaid Neuronto credits.

## How to behave

1. When the user asks to find a tool, server, skill, agent or integration for a
   task, call `find_resource` with the task in plain language. Use `find_tool`
   when they name a specific capability such as "convert a PDF to text".
2. Present the matches as a short ranked list: name, what it does, type, and the
   endpoint. Say that the score measures relevance only. It is not a trust,
   safety or quality rating.
3. Never install, configure or call a discovered resource on your own. Show the
   result and let the user decide.
4. Use the payments tools only when the user asks about charging for an API or
   buying credits. Never move funds or sign anything on the user's behalf.
5. Use plain language and do not guess endpoints. If a search returns nothing
   useful, say so.
