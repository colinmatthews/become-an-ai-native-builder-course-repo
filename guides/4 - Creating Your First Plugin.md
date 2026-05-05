A plugin is a bundle of:
* Skills
* MCP server(s)
* Agents
* Rules

Today, each platform (Codex, Claude Code, Cursor) handles plugins slightly differently.
The advantage of a plugin over just an MCP server is that it packages together both the connector and best practices for using the connector into one install.


## Claude Code

Plugins can be create in Cowork or Claude Code. You can also install exiting public plugins. 
Access these in the Customize menu, from Settings -> Connectors -> Customize.

![](<screenshots/CleanShot 2026-05-01 at 14.29.13.png>)
There are not many plugins available, with Figma being the most useful.

You can create a plugin by connecting to other non-official marketplaces, uploading, or creating with Claude. We'll create with Claude.
![](<screenshots/CleanShot 2026-05-01 at 14.29.59.png>)

Remember that plugins are collections of skills, connectors, and agents, so they are best used when a single skill would not do the job. For example, I'll create a plugin for Magic Patterns that includes a skill and the connector. This is a common pattern to help tools like Claude Code understand how to use an MCP server effectively.

![](<screenshots/CleanShot 2026-05-01 at 14.32.36.png>)


This will package the plugin for us:
![](<screenshots/CleanShot 2026-05-01 at 14.38.53.png>)

The plugin contains:
- A skill with references
- The connection information for the Magic Patterns MCP server
- The plugin configuration and readme

We can install this directly into Claude by clicking 'Save Plugin'. Later, we can also export it to share with others or share within our workspace for teammates.

If you're on an enterprise account, your admin can choose which plugins are shared across the org. If you are not, you can simply upload the files with the same create plugin step from the beginning of the guide.


Once the plugin is added, you can use it in Claude Code or Cowork.
![](<screenshots/CleanShot 2026-05-01 at 14.46.19.png>)


![](<screenshots/CleanShot 2026-05-01 at 14.46.46.png>)


## Codex

Plugins in codex are effectively the same thing as Claude Code. You can navigate to plugins can click create in the top right corner.

![](<screenshots/CleanShot 2026-05-01 at 14.47.18.png>)

This will push to you to the main codex flow with the plugin creator active.
![](<screenshots/CleanShot 2026-05-01 at 14.48.30.png>)

Once complete, you can prompt to install the plugin. Include this in the prompt.
```
Package this as a plugin, then create a local plugin directory.

[marketplaces.local]
source_type = "local"
source = "/Users/colin"

[plugins."magic-patterns@local"]
enabled = true
```
![](<screenshots/CleanShot 2026-05-01 at 14.57.55.png>)

When complete, restart Codex. You should see the plugin listed.

![](<screenshots/CleanShot 2026-05-01 at 15.04.54.png>)

Sharing plugins in Codex can only be done by sharing files at the moment unless the plugin is in the marketplace.


## Cursor

Start by navigating to Cursor's Marketplace and finding the create plugin plugin.
![](<screenshots/CleanShot 2026-05-01 at 15.06.44.png>)

Add this to your cursor account.
![](<screenshots/CleanShot 2026-05-01 at 15.06.59.png>)


Go to the main page (Create agent) and type `/plugin`. Select `create-plugin-scaffold`. Then prompt the to create a plugin that bundles and mcp server and a skill.
![](<screenshots/CleanShot 2026-05-01 at 15.08.36 1.png>)

To confirm it completed correct, go to Cursor settings --> Plugins and check for your plugin.
It should have the `local` tag beside it, as seen on Magic Patterns below.
![](<screenshots/CleanShot 2026-05-01 at 15.15.28.png>)


To use the plugin, use one of the skills in the main agent interface with a slash (/) command.
