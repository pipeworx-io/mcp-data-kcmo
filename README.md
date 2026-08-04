# mcp-data-kcmo

DataKansasCityMO MCP — Kansas City, MO open data (data.kcmo.org, Socrata SODA API).

Part of [Pipeworx](https://pipeworx.io) — an MCP gateway connecting AI agents to 1394+ live data sources.

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

Or connect to the full Pipeworx gateway for access to all 1394+ data sources:

```json
{
  "mcpServers": {
    "pipeworx": {
      "url": "https://gateway.pipeworx.io/mcp"
    }
  }
}
```

## Using with ask_pipeworx

Instead of calling tools directly, you can ask questions in plain English:

```
ask_pipeworx({ question: "your question about Data Kcmo data" })
```

The gateway picks the right tool and fills the arguments automatically.

## More

- [Docs and guides](https://pipeworx.io/docs)
- [pipeworx.io](https://pipeworx.io)

## License

MIT
