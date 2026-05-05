The course has it's own MCP server to turn your AI agent into a course assistant.

Here's how you get setup:

## Claude Code

Start by opening your Settings in Claude Code.
![](<screenshots/CleanShot 2026-05-02 at 13.41.54.png>)

Open the Customize menu and click "Add custom connector"
![](<screenshots/CleanShot 2026-05-02 at 13.42.36.png>)


Add a connector with the name "Become an AI-Native Builder" with the URL https://bold-forge-rhvty.run.mcp-use.com/mcp.

![](<screenshots/CleanShot 2026-05-02 at 13.45.32.png>)

The successful connection should look like this:
![](<screenshots/CleanShot 2026-05-02 at 13.47.41.png>)

To use the MCP, just prompt and mention the connector or the course, eg:
>What is the difference between a skill and plugin? Check the ai native builder mcp

Here's an example result:
![](<screenshots/CleanShot 2026-05-02 at 13.50.06.png>)


## Codex
Go to settings --> MCP in Codex and add a new MCP. Select Streamable HTTP and fill in the MCP with the name "Become an AI-Native Builder" with the URL https://bold-forge-rhvty.run.mcp-use.com/mcp.

![](<screenshots/CleanShot 2026-05-02 at 13.51.55.png>)

Restart Codex at this point. Then, type /mcp to verify the connection is available.
![](<screenshots/CleanShot 2026-05-02 at 13.53.56.png>)

To use the MCP, just prompt and mention the connector or the course, eg:
>What is the difference between a skill and plugin? Check the ai native builder mcp

Here's an example result:
![](<screenshots/CleanShot 2026-05-02 at 13.55.48 1.png>)



## Cursor

Go to Cursor settings (gear icon bottom left corner) and MCP settings.
![](<screenshots/CleanShot 2026-05-02 at 13.56.29.png>)

Add the following to your MCP file configuration:

```
{
  "mcpServers": {
    "Become an AI-native builder": {
      "url":"https://bold-forge-rhvty.run.mcp-use.com/mcp",
    }
  }
}
```

If you already have MCP servers, you can just add this section with a comma on the prior MCP server.

```
{
  "mcpServers": {
    "Example MCP":{
      "url":"https://made-up-mcp.notreal/mcp
    },
    "Become an AI-native builder": {
      "url":"https://bold-forge-rhvty.run.mcp-use.com/mcp",
    }
  }
}
```

Once connected, you should see a green icon in Cursor beside the MCP.
![](<screenshots/CleanShot 2026-05-02 at 14.02.08.png>)


You can then prompt to use the MCP server:
>What is the difference between a skill and plugin? Check the ai native builder mcp

![](<screenshots/CleanShot 2026-05-02 at 14.02.54.png>)


