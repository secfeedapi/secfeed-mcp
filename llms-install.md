# Installing the SecFeed MCP server

SecFeed is a remote (hosted) MCP server. There is nothing to clone, build or run.

1. Ask the user for their SecFeed API key. If they do not have one, tell them to create one at https://secfeedapi.com/signup/ and copy it from https://secfeedapi.com/dashboard/.
2. Add this entry to the MCP settings file (for Cline: `cline_mcp_settings.json`), replacing YOUR_KEY with the key:

```json
{
  "mcpServers": {
    "secfeed": {
      "type": "streamableHttp",
      "url": "https://secfeedapi.com/mcp",
      "headers": { "Authorization": "Bearer YOUR_KEY" }
    }
  }
}
```

3. Verify: the server should list 28 tools. Call `map_ticker` with `{"ticker": "AAPL"}`; it returns CIK 320193, Apple Inc.

If tool calls return 401, the key is missing or wrong. If they return 429, the plan's call quota is used up (see https://secfeedapi.com/dashboard/).
