# Claude
## Integrated MCP Apps
Navigate to the Claude desktop under Settings and Connectors.
![[CleanShot 2026-04-28 at 15.02.08.png]]

Click the button at the top to navigate to Browse Connectors.

![[CleanShot 2026-04-28 at 15.03.56.png]]Search for the product you want to connect (eg: Magic Patterns). Click add. 

![[CleanShot 2026-04-28 at 15.04.44.png]]
This will open a temporary browser window, which will close and display a success screen once connected.

![[CleanShot 2026-04-28 at 15.06.00.png]]
You can then prompt to use Magic Patterns in any chat.


## Non-Integrated MCP Apps

![[CleanShot 2026-04-28 at 15.06.42.png]]

Navigate to MCP settings and click the link to go to 'Customize'

![[CleanShot 2026-04-28 at 15.07.17.png]]
Click add and select a Custom Connector.


![[CleanShot 2026-04-28 at 15.07.56.png]]

Add the name and URL of the MCP server to connect. Optionally provide static oAuth parameters these values are required (typically for internal MCPs).

# Codex

## Integrated MCP Apps

Navigate to Plugins on the left panel.
![[CleanShot 2026-04-28 at 15.09.00.png]]

Find the product you want to connect to and click 'Add'.

![[CleanShot 2026-04-28 at 15.09.38.png]]

This will open a webpage to confirm connection.
![[CleanShot 2026-04-28 at 15.09.51.png]]


Once connected, you will get a confirmation screen.
![[CleanShot 2026-04-28 at 15.10.22.png]]****


## Non-Integrated MCP Apps

### oAuth Apps

Follow the steps here to install the Codex CLI: https://developers.openai.com/codex/cli

Open a terminal on your computer. If you're unsure if you have Node installed, run `node -v` in your terminal.
![[CleanShot 2026-04-28 at 14.44.43.png]]

If this returns a node version number, you can continue. Otherwise, please refer to the getting started guide.

Run the command `npm i -g @openai/codex`. This will install the Codex CLI. 

![[CleanShot 2026-04-28 at 14.49.01.png]]
Run the command `codex mcp add magicpatterns --url=https://mcp.magicpatterns.com/mcp`  to add the Magic Patterns MCP.

Next, login with `codex mcp login magicpatterns`.  This will open a browser window asking you to connect your Magic Patterns account to Codex.
![[CleanShot 2026-04-28 at 14.51.18.png]]


Now we can move over to the Codex desktop app. 
Prompt to ask if Codex can see the Magic Patterns tools, and for it to list them.

![[CleanShot 2026-04-28 at 14.56.14.png]]

Ask Codex to create a design, like a button. You should see a permissions screen to allow use of the tool, which you can accept. You may want to 'Always allow' if you do not want to be asked again in the future.

![[CleanShot 2026-04-28 at 14.57.25.png]]

This should generate a design in Magic Patterns.

### Token-based Auth App

Token based apps are a lot easier. Navigate to the Codex desktop app MCP settings, and click add MCP server.

![[CleanShot 2026-04-28 at 14.59.51.png]]


Select Streamable HTTP, enter the MCP server URL, and your tokens in the below sections for bearer auth, headers, and env values.
![[CleanShot 2026-04-28 at 15.00.07.png]]


# Cursor
Navigate to Cursor settings in the top bar.
![[CleanShot 2026-04-28 at 15.11.46.png]]


Select tools and MCP.
![[CleanShot 2026-04-28 at 15.12.22.png]]

Click add new MCP server. This will open a text file to edit. Edit this configuration to add the MCP server of your choice. For example, Magic Patterns MCP is:
`{ "mcpServers": { "magic-patterns": { "url": "https://mcp.magicpatterns.com/mcp" } } }`

![[CleanShot 2026-04-28 at 15.12.41.png]]

Once added, navigate back to settings. You'll see that the MCP server requires authentication.
![[CleanShot 2026-04-28 at 15.14.29.png]]

Click connect to open a webpage and approve connection.
![[CleanShot 2026-04-28 at 15.15.01.png]]

When complete, the MCP server should be listed green and show the available tools.

![[CleanShot 2026-04-28 at 15.15.25.png]]
