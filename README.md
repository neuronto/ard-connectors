# ARD connectors

Client-side connectors (Skills + MCP) that let an AI client discover agentic
resources through **ARD (Agentic Resource Discovery)**: the MCP servers, skills,
agents and APIs that can actually do a task.

Upstream project: [ards-project/ard-connectors](https://github.com/ards-project/ard-connectors).
This distribution is maintained by [Neuronto](https://neuronto.com) and differs
in two ways: it ships as an installable Claude Code plugin marketplace, and the
Agent Finder list contains **every public ARD discovery service** rather than a
subset, so the menu reflects the whole ecosystem.

## Claude Code

```
/plugin marketplace add neuronto/ard-connectors
/plugin install agentfinder@ard-connectors
```

The plugin bundles the `neuronto` MCP connector, so the skill can query the
federated index straight away with no further setup. Removing or replacing it is
a one-line edit to `.mcp.json`.

Then ask for what you need in plain language:

```
/agentfinder a tool that can read PDFs
```

The skill shows the Agent Finder menu once, remembers your choice in
`~/.agentfinder/finders.json`, presents ranked results, and **never installs
anything for you**. Installation is always your explicit step.

## Other clients

MCP connector configs, one per client, in [`mcp/`](mcp/):

| Client | File |
|---|---|
| Claude Desktop | [`mcp/claude/claude_desktop_config.json`](mcp/claude/claude_desktop_config.json) |
| Cursor | [`mcp/cursor/mcp.json`](mcp/cursor/mcp.json) |
| GitHub Copilot / VS Code | [`mcp/copilot/mcp.json`](mcp/copilot/mcp.json) |
| Gemini | [`mcp/gemini/settings.json`](mcp/gemini/settings.json) |
| Google Antigravity | [`mcp/google-antigravity/mcp_config.json`](mcp/google-antigravity/mcp_config.json) |
| ChatGPT | [`mcp/chatgpt/README.md`](mcp/chatgpt/README.md) |

Client instruction files (rules, gems, custom instructions) are in
[`skills/`](skills/) for clients that do not support MCP.

Each config points at a single endpoint. Change the URL to any finder in
[`agent-finders.json`](agent-finders.json); the request shape is the same,
because they all implement the same ARD search interface.

## The Agent Finders

Every public ARD discovery service known at the time of writing, alphabetical,
none preselected. Full details in [`agent-finders.json`](agent-finders.json).

| Finder | Search endpoint | Notes |
|---|---|---|
| ARD Registry | `ardregistry.org/api/search` | serves search at `/api/search`, not the spec's `/search` |
| Desvela ARD Registry | `registry.desvela.dev/search` | also offers domain preflight tooling |
| GitHub Agent Finder | `agentfinder.github.com/api/v1/search` | GitHub's catalog of installable resources |
| Hugging Face Discover | `huggingface-hf-discover.hf.space/search` | indexes Hugging Face Spaces, Skills, MCP cards |
| Neuronto | `neuronto.com/search` | federated: queries the others and fuses the rankings |
| WellKnown | `wellknownhq.com/registry/search` | census of well-known manifests |

Add your own by appending to the `finders` array.

## The search interface

Every finder answers the same POST:

```http
POST <finder search URL>
Content-Type: application/json

{ "query": { "text": "read a PDF and extract tables" } }
```

Narrow by resource type:

```json
{ "query": { "text": "...", "filter": { "type": ["application/mcp-server+json"] } } }
```

Results carry a `score`. It is **relevance only** and is not a trust, safety or
quality rating. Do not present it as one.

## Contributing

Fixes that belong to everyone should go to
[upstream](https://github.com/ards-project/ard-connectors). Issues specific to
this distribution, or a new Agent Finder to list, are welcome here.

## Licence

**Unresolved, and stated plainly rather than guessed.** The upstream repository
publishes no licence: no `LICENSE` file, no licence field, no statement in its
README. Under default copyright that means no redistribution terms are granted
for the files inherited from it, so this fork cannot assert any either. (The ARD
specification repository is Apache-2.0; the connectors repository is not.)

Changes made in this fork are offered under Apache-2.0 by their author. That does
not extend to the inherited files. Asking upstream to add a licence is the clean
fix and is worth doing for everyone who forks it, not only for us.
