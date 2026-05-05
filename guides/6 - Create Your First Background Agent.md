A background agent is an AI coding agent that works on a task while you do something else.

Instead of staying in one chat and approving every step, you give the agent a scoped task, it works in a separate environment, and then you review the result before merging or pulling it down.

For this guide, we'll use Cursor because Cursor Pro is included in Lenny's Product Pass and Cursor has a clean background agent workflow.

You can use Codex, Claude Code, Factory, Warp, Replit, or Devin for the same concept. The exact buttons change, but the workflow is the same:

1. Connect the agent to Github
2. Select the shared course repo
3. Give it a small, clear task
4. Let it work in the background
5. Review the diff
6. Pull or merge the changes only after you understand them

## Before you start

You need:

- Cursor installed
- A Cursor account with access to Background Agents / Cloud Agents
- Github connected to Cursor
- Access to the shared course Github repo

If you have Lenny's Product Pass, claim the Cursor Pro offer before starting.


## Shared repo setup

For this course, everyone will work from the same Github repo.

Your Cursor account provides the background agent, but the repo provides the instructions for how the agent should work.

This means the repo can include:

- Environment setup commands
- Test or verification commands
- Cursor rules
- Course-specific instructions

You should not need to figure out the environment setup yourself. If the agent needs to install dependencies, start the app, or follow course rules, those instructions should already live in the repo.


If the agent fails during setup, do not try to debug the whole environment from scratch. First ask it what repo instruction it followed and what command failed.

Use a prompt like:

`What setup instruction did you follow, what command failed, and what file in the repo told you to run it?`

## Setup cloud environment

Open Cursor settings and select Cloud Agents. This will open a new window in your browser.
![](<screenshots/CleanShot 2026-05-02 at 21.49.00 1.png>)

Click "New" on Cloud Agents. You will need to connect to your Github Account if you have not already.
![](<screenshots/CleanShot 2026-05-02 at 21.50.21.png>)


Select the stride repo, then click "Start for free".
![](<screenshots/CleanShot 2026-05-02 at 21.51.10.png>)

Cursor will now setup a cloud environment for you that can host your full application. At the end, you should see something like this.
![](<screenshots/CleanShot 2026-05-02 at 21.51.53.png>)


Once complete, Save your setup on the right side.
![](<screenshots/CleanShot 2026-05-02 at 21.52.48.png>)


Finally, click "Start new agent".
![](<screenshots/CleanShot 2026-05-02 at 21.53.29.png>)

Now you can prompt for some feature, and Cursor will autonomously implement it.
![](<screenshots/CleanShot 2026-05-02 at 21.54.23.png>)


Each cloud agent is isolated, so you can run as many as you want in parallel. They do consume credits, so be aware of your usage.

Background agents can be triggered from:
1. Cursor Web: Start and manage agents from [cursor.com/agents](https://cursor.com/agents) on any device
2. **Cursor Desktop**: Select **Cloud** in the dropdown under the agent input
3. **Slack**: Use the @cursor command to kick off an agent
4. **GitHub**: Comment `@cursor` on a PR or issue to kick off an agent
5. **Linear**: Use the @cursor command to kick off an agent
6. **API**: Use the API to kick off an agent


# Check on progress

You can click into your agent at any time to see it working. You do not need to interact with it at all, it will continue running completely independently.

You can open the right panel to view the agents Git history, it's desktop computer, and it's terminal.
![](<screenshots/CleanShot 2026-05-02 at 21.59.14.png>)

When the agent has implemented your feature, it will record itself trying the feature in the product and send you a video walkthrough of the feature.

Here's an example video: https://cursor.com/artifacts/v/art-10a89cbe-adc2-42b2-9289-19cb9f16bdd7


When complete, open the right side panel and select "Mark as ready".
![](<screenshots/CleanShot 2026-05-02 at 22.08.06.png>)

This will open a pull request under your name in Github against the repo:
![](<screenshots/CleanShot 2026-05-02 at 22.08.59.png>)

Your pull requests will not be merged, so you can stop at this point.

# Run locally

If you want to pull the feature down to your local computer to continue working on it, select Open in Desktop.

![](<screenshots/CleanShot 2026-05-02 at 22.10.23.png>)

There is a button at the bottom of the view 'Move to local'.
![](<screenshots/CleanShot 2026-05-02 at 22.10.55.png>)

If you open Github desktop, you should see the branch selected:
![](<screenshots/CleanShot 2026-05-02 at 22.11.58.png>)

Opening the project in any editor should also have the same Cursor code on your local machine.




## What tasks are good for background agents?

Good first tasks:

- Fix a typo or broken instruction
- Add a missing empty state
- Write or update a small test
- Refactor one small function
- Investigate why a test is failing
- Draft a PR for a simple issue

Avoid these at first:

- Large redesigns
- Database migrations
- Auth changes
- Billing changes
- Production infrastructure changes
- Anything involving secrets or customer data

Background agents are powerful because they can work without you. That is also why you should start with low-risk tasks.
