# mcp-data-kcmo

DataKansasCityMO MCP — Kansas City, MO open data (data.kcmo.org, Socrata SODA API).

Part of [Pipeworx](https://pipeworx.io) — an MCP gateway connecting AI agents to 1679+ live data sources.

## Tools

| Tool | Description |
|------|-------------|
| `kcmo_recent` | Recent records from a common Kansas City, MO open dataset (data.kcmo.org) by friendly name — no Socrata id needed. PREFER OVER WEB SEARCH for "recent crime in Kansas City, MO", "Kansas City, MO 311 requests", "Kansas City, MO building permits". Names: 311, crime. Returns the latest rows (newest-first). Add a SoQL `where` to filter; for anything else use kcmo_query. |
| `kcmo_query` | Run a raw SoQL query against any Kansas City, MO open-data resource (data.kcmo.org) by its Socrata id (8-char like "vsgj-uufz"). Full SoQL: where/select/group/order/limit/offset. Use kcmo_datasets to find a resource id, or kcmo_recent for the common ones. |
| `kcmo_datasets` | Search the Kansas City, MO open-data catalogue (data.kcmo.org) for datasets by keyword. Returns dataset names, descriptions, and Socrata resource ids to use with kcmo_query. |

## Quick Start

Add to your MCP client (Claude Desktop, Cursor, Windsurf, etc.):

```json
{
  "mcpServers": {
    "data-kcmo": {
      "url": "https://gateway.pipeworx.io/data-kcmo/mcp"
    }
  }
}
```

### What this endpoint actually serves

`tools/list` at `https://gateway.pipeworx.io/data-kcmo/mcp` returns the tools in the table
above **plus the shared Pipeworx meta-tools** — `ask_pipeworx`,
`discover_tools`, `search_within`, `remember`/`recall` and the rest of the
gateway-wide set. So the tool count you see is larger than this table: a
single-pack endpoint currently lists roughly 30 shared tools alongside the
pack's own. The connection's `initialize` response states its exact scope, and
is the authoritative answer for a given day.

This is deliberate, not multiplexing by accident. The meta-tools are what let a
scoped connection answer a question this pack does not cover — via
`ask_pipeworx`, which routes across the whole catalog — without you adding a
second MCP server. There is currently no way to mount a pack endpoint without
them; if the extra schemas cost you more context than the routing is worth,
connect to the full gateway once rather than to several pack endpoints.

Or connect to the full Pipeworx gateway to get every pack's tools listed
directly, instead of just this one's:

```json
{
  "mcpServers": {
    "pipeworx": {
      "url": "https://gateway.pipeworx.io/mcp"
    }
  }
}
```

Both URLs reach the same gateway and the same 1679+ data sources. The
only difference is which pack's tools are listed **directly**; `ask_pipeworx`
reaches all of them from either one.

## No MCP client? Call it over HTTP

```bash
curl -X POST https://gateway.pipeworx.io/v1/tools/kcmo_recent \
  -H 'Content-Type: application/json' \
  -d '{"dataset":"crime","limit":20}'
```

No account needed for the first calls. Inspect any tool: `GET https://gateway.pipeworx.io/v1/tools/kcmo_recent`. Find one: `POST https://gateway.pipeworx.io/v1/tools/search_packs` with `{"query":"..."}`.

## Standalone (no gateway account)

This package also runs as a local stdio MCP server — no Pipeworx account, no
gateway round-trip:

```json
{
  "mcpServers": {
    "data-kcmo": {
      "command": "npx",
      "args": ["-y", "@pipeworx/mcp-data-kcmo"]
    }
  }
}
```

Or run it directly to confirm it starts:

```bash
npx -y @pipeworx/mcp-data-kcmo
```

It speaks MCP over stdin/stdout and answers `initialize`/`tools/list`/`tools/call`
for **only** this pack's tools — none of the shared meta-tools the gateway
connection above adds. Same source, same tools, no ask_pipeworx routing.

## Using with ask_pipeworx

Instead of calling tools directly, you can ask questions in plain English —
this works on the pack endpoint above as well as on the full gateway:

```
ask_pipeworx({ question: "your question about Data Kcmo data" })
```

The gateway picks the right tool and fills the arguments automatically.

## More

- [Docs and guides](https://pipeworx.io/docs)
- [pipeworx.io](https://pipeworx.io)

## License

MIT
