
Before you start, please create a copy of the Stride repo [here](https://github.com/colinmatthews/stride)
![](<screenshots/CleanShot 2026-05-03 at 12.52.43.png>)

## Cursor
You must have a Cursor Pro account to follow these guidelines.

To start, navigate to your settings in the Cursor web app (not desktop) and find the setup instructions. Link your Github account.
![](<screenshots/CleanShot 2026-05-03 at 12.19.57.png>)

Go to your cloud agent settings and set your default repo to the repo you just created.
![](<screenshots/CleanShot 2026-05-03 at 12.53.45.png>)

Then go to Issues and create a new issue. Then tag @cursoragent in a follow up comment with some prompt (eg: Take a look).
![](<screenshots/CleanShot 2026-05-03 at 12.57.35.png>)

Cursor will reply with a button to open a PR. Click it!
![](<screenshots/CleanShot 2026-05-03 at 12.59.04.png>)


The PR will show the name of your branch. You can either continue chatting with Cursor to make changes or pull the branch locally. 
![](<screenshots/CleanShot 2026-05-03 at 12.59.57.png>)

Note that without setting up a cloud environment in Cursor settings, Cursor cannot run the desktop to interact with your product. It can only make code changes.

You can also add Cursor to Slack (more [here](https://cursor.com/docs/integrations/slack)).

You can also create background agents any time from the desktop app by selecting 'Cloud' instead of 'Local'.
![](<screenshots/CleanShot 2026-05-03 at 13.22.06.png>)


## Claude Code
Installing the Claude Code background agent integration with Github only works via the terminal.
If you wish to install it, open a terminal and run `/install-github-app`.  Make sure you are in the folder of the project you cloned.
![](<screenshots/CleanShot 2026-05-03 at 13.05.31.png>)

This will run through a number of changes on your github repository. Generally I would not recommend this without permission from your engineering team, as it modifies Github Actions for all users.

Continue to accept changes on the web, then go back to the terminal.
![](<screenshots/CleanShot 2026-05-03 at 13.08.42.png>)

You can decide if you want code review automatically on PRs or just tagging. Use space to deselect an item and enter to continue.
![](<screenshots/CleanShot 2026-05-03 at 13.09.18.png>)

Create a long-lived access token with your subscription, do not use the API. The API will cost additional credits outside your subscription.

When complete, you should see this screen.
![](<screenshots/CleanShot 2026-05-03 at 13.10.36.png>)

This will open a new browser tab with a prefilled pull request. Create the PR then merge it.
![](<screenshots/CleanShot 2026-05-03 at 13.12.13.png>)

You can now mention @claude on github issues to get changes made with a background agent.
![](<screenshots/CleanShot 2026-05-03 at 13.13.58.png>)


You can also add Claude Code to Slack (more [here](https://code.claude.com/docs/en/slack))

You can also create background agents by selecting 'Cloud' instead of 'Local'.
![](<screenshots/CleanShot 2026-05-03 at 13.23.03.png>)

## Codex

Go to https://chatgpt.com/codex/cloud and select to configure your repo.
![](<screenshots/CleanShot 2026-05-03 at 13.19.50.png>)

Install the ChatGPT connector.
![](<screenshots/CleanShot 2026-05-03 at 13.20.21.png>)

Go back to Codex and select the repo you copied:
![](<screenshots/CleanShot 2026-05-03 at 13.20.39.png>)

Add a prompt!
![](<screenshots/CleanShot 2026-05-03 at 13.21.04.png>)


You can also use background agents by selecting 'Cloud' instead of 'Local'.
![](<screenshots/CleanShot 2026-05-03 at 13.23.19.png>)
