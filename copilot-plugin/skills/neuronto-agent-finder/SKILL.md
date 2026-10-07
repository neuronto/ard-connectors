---
name: neuronto-agent-finder
description: Find MCP servers, skills, agents and APIs that can do a task by searching the Neuronto ARD Registry, which also covers the other public ARD registries. Use when the user wants to find, compare or choose a tool, server, skill, agent or API for something they are trying to do. Presents ranked results and never installs anything automatically.
---

# Find agentic resources

Use the `neuronto-ard-registry` MCP server bundled with this plugin to find
resources for a task.

## Steps

1. Take the task in the user's own words. Call `find_resource` with it. If the
   user names a specific capability, such as "convert a PDF to text", call
   `find_tool` as well, which searches individual MCP tools rather than servers.
2. Show a short ranked list. For each match give its name, what it does, its
   type, and the endpoint to connect to.
3. State that the score measures relevance only. It is not a trust, safety or
   quality rating.
4. Stop there. Never install, configure or call a discovered resource without
   the user's explicit instruction.
5. If nothing relevant comes back, say so. Do not invent endpoints.

## Other tools

- `registry_stats` reports how large the index is and how much of it has been
  verified.
- `publish_resource` lists an MCP server or an ARD-publishing domain. Use it
  only when the user asks to publish.
- The `neuronto-payments` server offers `monetize_api`, `integration_snippet`,
  `facilitator_status`, `settlement_price`, `find_paid_resource` and
  `buy_credits`. Use them only when the user asks about charging for an API or
  buying credits. Never move funds or sign anything for the user.
