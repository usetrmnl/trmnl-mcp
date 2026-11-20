# TRMNL MCP server

Run your [TRMNL](https://trmnl.com) ePaper displays from any AI agent that speaks the Model Context Protocol (MCP): devices, playlists, plugins and new screens.

The server is hosted by TRMNL. There is nothing to install or run. This repository holds the description that MCP registries read; the server code lives in the TRMNL web app.

- Endpoint: `https://trmnl.com/mcp` (streamable HTTP)
- Registry name: `com.trmnl/trmnl`
- Setup for each client (Claude, ChatGPT, Cursor and others): https://help.trmnl.com/en/articles/17432548-mcp-server

## Connect

There are two ways to sign in. Each opens a different set of tools.

### Whole account (OAuth)

Add `https://trmnl.com/mcp` as a remote MCP server. Your client opens the TRMNL sign-in page, and you pick what the agent may do. You can change or revoke that later under Account, Connected agents.

```json
{
  "mcpServers": {
    "trmnl": {
      "type": "http",
      "url": "https://trmnl.com/mcp"
    }
  }
}
```

### One plugin (MCP key)

On a plugin's settings page in the TRMNL dashboard, click "Generate MCP Key". The key reaches that one plugin only. "Revoke Key" on the same page cuts off access straight away.

```json
{
  "mcpServers": {
    "trmnl": {
      "type": "http",
      "url": "https://trmnl.com/mcp?api_key=${TRMNL_MCP_API_KEY}"
    }
  }
}
```

## What an agent can do

With an account sign-in, each tool is an operation of the [TRMNL REST API](https://trmnl.com/api-docs), in read and write pairs:

- Devices: list, claim, change settings, flash the screen to identify one, mirror another device
- Playlists: add, hide, reorder and remove screens
- Plugins and plugin settings: create private plugins, change their settings, send data
- Markup: read and write a plugin's HTML and Liquid for each screen size
- Recipes: search, read and install community plugins
- Themes, profile and apps

With a plugin MCP key:

- Markups: read, write, list sizes, screenshot, and list or restore earlier versions
- Integrations: show settings, write settings, refresh data, read logs
- Merge variables: show the data the markup renders from
- Recipes: search and pull markup from community recipes
- Design system: the TRMNL framework's rules, classes and examples

## Example

> Make a screen that shows today's sales from our warehouse on my TRMNL in the office.

The agent creates a private plugin, writes markup for each screen size, sends the numbers as merge variables, previews the result, and adds it to the device's playlist.

## Limits

- Each connection may send 60 requests a minute.
- A change that renders a screen counts against the account's hourly render allowance. Past it the server answers 429 until the hour is up.

## More

- Authentication for agents: https://trmnl.com/auth.md
- TRMNL CLI: https://github.com/usetrmnl/cli
- Support: https://help.trmnl.com
