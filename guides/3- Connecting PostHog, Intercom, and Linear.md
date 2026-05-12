For this course you'll connect Claude Code to three services: **Intercom**, **PostHog**, and **Linear**. Recall that we can add MCP connections as individual users (oAuth) or connect as a service with an token. In this guide, we'll be using token-based auth so you can log in to my existing Linear, Intercom, and PostHog workspaces without creating new accounts.


# Claude Code

To add MCP servers using bearer tokens, you'll need to use the Claude Code CLI. 
![](<screenshots/CleanShot 2026-05-01 at 11.08.44.png>)
(`claude --version` should print a version).


If not, install for your platform. The easiest way to to ask Claude Code desktop to help you install Claude CLI. Use this prompt:
`can you help me install claude code for terminal?`

If you want to install manully, full instructions are here: https://code.claude.com/docs/en/overview#native-install-recommended


## Step 1. Connect Intercom

The Intercom MCP server lets Claude read your support conversations, tickets, and contact data.

Copy and paste the command below to connect to our Intercom:
```bash
claude mcp add -s user --transport http intercom https://mcp.intercom.com/mcp \
--header "Authorization: Bearer dG9rOmFjYTE3MDQwXzY2YTJfNGQ5Y19hMjljX2QyNThmNTA1YTY0NToxOjA="
```

Then try this command to list your MCP servers:
```
claude mcp list
```

You should see your list of MCP servers, including Intercom with the connected status:
![](<screenshots/CleanShot 2026-05-01 at 11.12.06.png>)

Restart your Claude Desktop app and try the following prompt in Code:

> Using the Intercom MCP, show me 3 recent support conversations with their body text.

You should see a response similar to this:
![](<screenshots/CleanShot 2026-05-01 at 11.15.48.png>)



## Step 2. Connect PostHog

  The PostHog MCP server lets Claude query events, persons, cohorts, and insights. We'll follow almost identical steps to add Posthog to Claude Code.
  
```bash
claude mcp add -s user --transport http posthog https://mcp.posthog.com/mcp \
--header "Authorization: Bearer  phx_Nm6Ke2MPD2KioQoWUm8hneZZNjnGLVsV2LhtbaaPPsCURGF8"
```

Then try this command to list your MCP servers:
```
claude mcp list
```

You should see your list of MCP servers, including Posthog with the connected status:
![](<screenshots/CleanShot 2026-05-01 at 11.24.49.png>)


Restart your Claude Desktop app and try the following prompt in Code:
> Using the PostHog MCP, how many `user_registered` events happened in the last 90 days?

![](<screenshots/CleanShot 2026-05-01 at 11.44.23.png>)


## Step 3. Connect Linear

The Linear MCP server lets Claude read issues, projects, cycles, and labels.


```bash
claude mcp add -s user --transport http linear https://mcp.linear.app/mcp \
  --header "Authorization: Bearer lin_api_UBFWr8MEbfs4ftuENQxmI5Ka0tmb76Sj5QjYeOkG"
```

Then try this command to list your MCP servers:
```
claude mcp list
```

You should see your list of MCP servers, including Linear with the connected status:
![](<screenshots/CleanShot 2026-05-01 at 11.47.57.png>)


Restart your Claude Desktop app and try the following prompt in Code:
> List the 3 newest Linear issues:

![](<screenshots/CleanShot 2026-05-01 at 11.49.01.png>)

## Step 4. Cross-service test

Try the following prompt to use all three MCP servers at once:
  
> Using Intercom, find the most recent conversation that mention sync issues. Get the customer's email from that conversation. Then use PostHog to find that person's last 5 events. Then check Linear for any issues that reference the conversation ID.


FYI that Claude loads MCP servers progressively and may lie about not having access:
![](<screenshots/CleanShot 2026-05-01 at 11.51.52.png>)

You should get a final response similar to this:
![](<screenshots/CleanShot 2026-05-01 at 11.52.49.png>)



## Checking your setup
Useful commands any time:
```bash

claude mcp list # show all connected servers + status

claude mcp get intercom # show config for one server

claude mcp remove intercom -s user # disconnect a server (can re-add later)

```



# Codex

The easiest way to add token-based MCP servers with Codex is to manually edit a file called config.toml

## Step 1. Edit config.toml

Run codex at least once before trying to access this file. Then open any text editor and navigate to ` ~/.codex/config.toml`. If you don't know how, open a terminal and run `open ~/.codex`
This will open the directory for you in finder.

Paste the following values at the bottom of the config.toml file and save.
```
[mcp_servers.intercom]
url = "https://mcp.intercom.com/mcp"
http_headers = { Authorization = "Bearer dG9rOmQ0ZDNkY2U1XzYzNjZfNDNkOF9hN2YwXzJkY2JiODYyZTM1MToxOjA=" }

[mcp_servers.posthog]
url = "https://mcp.posthog.com/mcp"
http_headers = { Authorization = "Bearer phx_xjAHY4iifs5KwEQmYJB5doFbXC9mtRC5ASvCrDBzZqyujvpe" }

[mcp_servers.linear]
url = "https://mcp.linear.app/mcp"
http_headers = { Authorization = "Bearer lin_api_VxaVg6wTEmJPH3V7S4t3psl3MyAWdADcRAH6hStC" }

```

![](<screenshots/CleanShot 2026-05-01 at 14.06.29.png>)


Restart Codex and prompt
> Verify Posthog, linear, and intercom are working with a simple MCP test.

## Step 2. Cross-service test

Try the following prompt to use all three MCP servers at once:
  
> Using Intercom, find the most recent conversation that mention sync issues. Get the customer's email from that conversation. Then use PostHog to find that person's last 5 events. Then check Linear for any issues that reference the conversation ID.


FYI that Claude loads MCP servers progressively and may lie about not having access:
![](<screenshots/CleanShot 2026-05-01 at 11.51.52.png>)

You should get a final response similar to this:
![](<screenshots/CleanShot 2026-05-01 at 11.52.49.png>)



# Cursor

Cursor uses a JSON file called `mcp.json` for custom MCP servers. For this course, we'll add the three MCP servers globally so they are available from any Cursor project.

## Step 1. Edit mcp.json

Run Cursor at least once before trying to access this file. Then open any text editor and navigate to `~/.cursor/mcp.json`.

If you don't know how, open a terminal and run:
```bash
open ~/.cursor
```

If the `.cursor` folder or `mcp.json` file does not exist yet, create them.

Paste the following values into `mcp.json` and save:
```json
{
  "mcpServers": {
    "intercom": {
      "url": "https://mcp.intercom.com/mcp",
      "headers": {
        "Authorization": "Bearer dG9rOmQ0ZDNkY2U1XzYzNjZfNDNkOF9hN2YwXzJkY2JiODYyZTM1MToxOjA="
      }
    },
    "posthog": {
      "url": "https://mcp.posthog.com/mcp",
      "headers": {
        "Authorization": "Bearer phx_xjAHY4iifs5KwEQmYJB5doFbXC9mtRC5ASvCrDBzZqyujvpe"
      }
    },
    "linear": {
      "url": "https://mcp.linear.app/mcp",
      "headers": {
        "Authorization": "Bearer lin_api_VxaVg6wTEmJPH3V7S4t3psl3MyAWdADcRAH6hStC"
      }
    }
  }
}
```

Restart Cursor after saving the file, then navigate to Cursor settings:
![](<screenshots/CleanShot 2026-05-01 at 14.12.46 1.png>)

You should see all three MCP servers connected:
![](<screenshots/CleanShot 2026-05-01 at 14.13.22.png>)


## Step 2. Cross-service test

Try the following prompt to use all three MCP servers at once:

> Using Intercom, find the most recent conversation that mention sync issues. Get the customer's email from that conversation. Then use PostHog to find that person's last 5 events. Then check Linear for any issues that reference the conversation ID.

Cursor loads MCP tools when the Agent decides they are relevant. If it says it cannot access a tool, make sure the server is enabled in Cursor's MCP settings, then start a fresh Agent chat and try again.

You should get a response similar to this:

![](<screenshots/CleanShot 2026-05-01 at 14.18.02.png>)


---

# Troubleshooting

## PostHog or Linear tools not appearing in Claude Code Desktop

**Symptom:** `claude mcp list` shows PostHog and Linear as `✓ Connected`, but in the Desktop app Claude only offers to "authenticate" instead of actually querying data.

**Cause:** Claude Code Desktop caches which MCP servers require OAuth authentication. If PostHog or Linear get added to this cache, the Desktop app intercepts them and surfaces OAuth wrapper tools instead of the real data tools — even when you've configured a valid bearer token.

**Fix:** Clear the entries from the auth cache and restart Desktop.

1. Open a terminal and run:
```bash
open ~/.claude/mcp-needs-auth-cache.json
```

2. Remove the `"posthog"` and/or `"linear"` entries from the JSON, leaving any other entries (e.g. Slack, Gmail) intact. The file should look something like:
```json
{"claude.ai Slack":{"timestamp":...,"id":"..."},"claude.ai Gmail":{"timestamp":...,"id":"..."}}
```

3. Save the file and restart Claude Code Desktop.

The bearer token will now be used directly and the full tool set will appear.

> **Note:** The bearer token approach works seamlessly in the Claude Code CLI. If you'd rather skip this fix, you can run cross-service prompts from a terminal session instead of the Desktop app.
