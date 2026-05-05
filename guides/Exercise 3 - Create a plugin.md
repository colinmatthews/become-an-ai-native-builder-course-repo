In Exercise 1 you connected the course MCP server. In Exercise 2 you wrote a skill that taught your agent how to use it well In this exercise you'll bundle them into a single plugin. A plugin packages an MCP server together with the skills, agents, and rules that make it useful. One install, everything wired up.

## What the plugin should contain

Your `course-mcp` plugin will bundle:

- **The connector** for the *Become an AI-Native Builder* MCP server (URL: `https://bold-forge-rhvty.run.mcp-use.com/mcp`)
- **The skill** you built in Exercise 2 — the one that enforces search → fetch → show_image, with help/feedback as escalations
- **A README** explaining what the plugin is for and which prompts trigger it

### Claude Cowork or Code

Prompt Claude to package together the skill and the MCP.

> Package together the /become-an-ai-builder skill and the Become an ai native builder mcp into a plugin 

![](<screenshots/CleanShot 2026-05-02 at 14.47.06.png>)


Click Save Plugin to install it directly.

![](<screenshots/CleanShot 2026-05-02 at 14.55.06.png>)

To use the plugin, open a new chat and select the plugin.
![](<screenshots/CleanShot 2026-05-02 at 14.55.43.png>)


### Codex

Navigate to Plugins in the left panel and click Create in the top-right corner.
![](<screenshots/CleanShot 2026-05-02 at 14.57.30.png>)

Prompt the following:
>  Package together the /become-an-ai-builder skill and the Become an ai native builder mcp into a plugin 

![](<screenshots/CleanShot 2026-05-02 at 14.58.00.png>)

Once complete, ask Codex to install the plugin.
Codex does not have clean install workflow and will to modify its own configuration, which can take many tool calls.

You will also have to prompt to make the plugin visible in the list of plugins after Codex completes the installation.

Finally, restart Codex and you should see your plugin available.
![](<screenshots/CleanShot 2026-05-02 at 15.06.25.png>)



### Cursor

Open Cursor's Marketplace and find the `create-plugin-scaffold` plugin. Add it to your account.

![](<screenshots/CleanShot 2026-05-02 at 15.06.48.png>)


In a new agent chat, type `/create-plugin-scaffold`, then prompt:

> Create a plugin called `course-mcp` that bundles:
>
> 1. The *Become an AI-Native Builder* MCP server at `https://bold-forge-rhvty.run.mcp-use.com/mcp`
> 2. The `course-mcp` skill from my Cursor skills directory
> 3. A README listing the trigger prompts and the call pattern


![](<screenshots/CleanShot 2026-05-02 at 15.07.48.png>)

Confirm the plugin is installed under Cursor settings → Plugins. It should have the `local` tag.

![](<screenshots/CleanShot 2026-05-02 at 15.10.50.png>)

