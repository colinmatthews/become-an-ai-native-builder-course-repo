Magic Patterns lets you move designs between Magic Patterns and AI coding tools through their remote MCP server:

```text
https://mcp.magicpatterns.com/mcp
```

You must create a Magic Patterns account before connecting the MCP. Magic Patterns says MCP access requires a paid plan, so use the Lenny's List Product Pass code if you have it.

If you are using a company subscription, you may be blocked from adding a new connector in some products. Using a personal subscription is recommended in this case, or you can add the MCP server from the command line. Refer to `1 - Setting Up Your First MCP Connection.md` for the general command line setup patterns.

Source: https://www.magicpatterns.com/docs/documentation/features/mcp-server/overview

## Claude Code

Go to Settings --> Connectors --> Browse and find Magic Patterns. Press Connect and move through the connection process.

![](<screenshots/CleanShot 2026-05-01 at 15.20.39.png>)

Magic Patterns' docs show the connector like this:

![[screenshots/magic-patterns-claude-connector.png]]

You should see this confirmation screen when complete.

![](<screenshots/CleanShot 2026-05-01 at 15.21.04.png>)

If the connector is not available in your Claude settings, use the CLI instead:

```bash
claude mcp add --transport http magic-patterns https://mcp.magicpatterns.com/mcp
```

Then run `/mcp` inside Claude Code and complete the Magic Patterns OAuth flow.

Test it with:

> Using the Magic Patterns MCP, list the available Magic Patterns tools.

# Codex

Codex can connect to Magic Patterns through the Codex CLI.

## Step 1. Add the MCP server

Open a terminal and run:

```bash
codex mcp add magic-patterns --url https://mcp.magicpatterns.com/mcp
```

Then confirm it was added:

```bash
codex mcp list
```

You should see `magic-patterns` listed as an enabled streamable HTTP MCP server.

## Step 2. Authenticate

Run:

```bash
codex mcp login magic-patterns
```

This should open a browser window where you can connect your Magic Patterns account. Complete the authorization flow, then restart Codex.

If `codex mcp login magic-patterns` says auth is unsupported, keep going. Some remote MCP servers only prompt for authentication when the agent first tries to call a tool.

## Step 3. Test in Codex

Open Codex and try:

> Using the Magic Patterns MCP, list the available Magic Patterns tools.

If Codex asks for permission to use the MCP server, approve the tool call.

If Codex says it cannot see Magic Patterns, run:

```bash
codex mcp get magic-patterns
```

Confirm the URL is `https://mcp.magicpatterns.com/mcp`. If it is missing, run the add command again and restart Codex.

# Cursor

Cursor uses an `mcp.json` file for custom MCP servers. Magic Patterns recommends adding the config to your project so Cursor can use Magic Patterns while working in that repo.

## Step 1. Add Magic Patterns to Cursor

Inside your project, create this file:

```text
.cursor/mcp.json
```

Paste this config:

```json
{
  "mcpServers": {
    "magic-patterns": {
      "url": "https://mcp.magicpatterns.com/mcp"
    }
  }
}
```

You can also do this from Cursor by opening Cursor Settings --> MCP & Tools and choosing Add new MCP server.

## Step 2. Enable the MCP server

Open Cursor Settings --> Cursor Settings --> MCP & Tools.

Make sure `magic-patterns` is turned on. Magic Patterns calls this out as important: if Cursor browses the web instead of using the MCP tools, the MCP is not actually working.

![[screenshots/magic-patterns-cursor-mcp-enabled.png]]

Cursor's MCP settings should show Magic Patterns in the tools list:

![[screenshots/magic-patterns-cursor-settings.png]]

## Step 3. Authenticate

If Cursor shows a Connect or Authenticate button for `magic-patterns`, click it and complete the Magic Patterns authorization flow in the browser.

When the setup is complete, `magic-patterns` should be enabled and its tools should be available in Cursor's Agent chat.

## Step 4. Test in Cursor

Open a new Agent chat in Cursor and try:

> Using the Magic Patterns MCP, list the available Magic Patterns tools.

