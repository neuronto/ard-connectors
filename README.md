# ARD connectors

Client-side connectors (Skills + MCP) that let an AI client discover agentic
resources through **ARD (Agentic Resource Discovery)**: the MCP servers, skills,
agents and APIs that can actually do a task.

ARD is also an IETF Internet-Draft since 30 October 2026, under the name **Agent
Resource Discovery** ([architecture](https://datatracker.ietf.org/doc/draft-bu-dawn-ard-architecture/),
[initial contact](https://datatracker.ietf.org/doc/draft-jimenez-dawn-ard-initial-contact/),
[manifest and lookup](https://datatracker.ietf.org/doc/draft-bu-dawn-ard-discovery/)).
The connectors speak both: the search request below is the same in the specification and
in the drafts.

Upstream project: [ards-project/ard-connectors](https://github.com/ards-project/ard-connectors).
This distribution is maintained by [Neuronto](https://neuronto.com) and differs
in two ways: it ships as an installable Claude Code plugin marketplace, and the
Agent Finder list contains **every public ARD discovery service** rather than a
subset, so the menu reflects the whole ecosystem.

## Claude Code

> Named `neuronto` rather than `ard-connectors` on purpose. Claude Code registers
> one marketplace per name and a second one with the same name **replaces** the
> first, so reusing upstream's name would silently evict it for anyone who had
> both installed. The plugin is `neuronto-agent-finder` for the same reason: the
> bare `agentfinder` name is upstream's and is what their site promotes.


```
/plugin marketplace add neuronto/ard-connectors
/plugin install neuronto-agent-finder@neuronto
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

## Gemini CLI

This repository is a Gemini CLI extension (`gemini-extension.json` and
`GEMINI.md` at the root). It adds both remote MCP servers:

```
gemini extensions install https://github.com/neuronto/ard-connectors
```

## GitHub Copilot

The plugin lives in [`copilot-plugin/`](copilot-plugin/): `plugin.json`,
`mcp.json` and one skill. It bundles the same two remote MCP servers. The plugin
path inside this repository is `copilot-plugin`.

## Cursor

The repository root is also a Cursor plugin (`.cursor-plugin/plugin.json` and
`mcp.json`). It bundles the same two remote MCP servers and the same skill as the
Copilot plugin.

## The two servers

| Server | Endpoint | What it does |
|---|---|---|
| Neuronto ARD Registry | `https://neuronto.com/mcp` | search for MCP servers, skills, agents, APIs and individual tools; publish a server to the index |
| Neuronto Payments | `https://pay.neuronto.com/mcp` | x402 facilitator on Base: put an API behind x402 and buy prepaid credits |

To add the payments server to Claude Code:

```
claude mcp add --transport http neuronto-payments https://pay.neuronto.com/mcp
```

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

Under the IETF drafts a finder may page with `pageSize` and `pageToken` (the response
carries `nextPageToken`), and answers errors as RFC 9457 Problem Details
(`application/problem+json`) with a code such as `INVALID_ARGUMENT` or
`RATE_LIMIT_EXCEEDED`. A finder's own manifest entry of type
`application/ai-registry+json` gives its base URL; append `/search`.

## Contributing

Fixes that belong to everyone should go to
[upstream](https://github.com/ards-project/ard-connectors). Issues specific to
this distribution, or a new Agent Finder to list, are welcome here.

## Licence

Files authored in this distribution are licensed under the Apache License 2.0
(see [`LICENSE`](LICENSE)). That covers `gemini-extension.json`, `GEMINI.md`,
everything in [`copilot-plugin/`](copilot-plugin/), `.cursor-plugin/`, `mcp.json`,
the Claude Code marketplace and plugin manifests, and this README.

The files inherited from the upstream repository are a separate matter. Upstream
publishes no licence for them (no `LICENSE` file, no licence field, no statement
in its README), so no terms are granted for those files by this repository.
They are `agent-finders.json`, `shared/`, `skills/` and the per-client configs in
`mcp/`. Asking upstream to add a licence is the clean fix for everyone who forks
it.
