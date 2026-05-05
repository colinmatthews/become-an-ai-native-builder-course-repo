In the last guide, Cursor created a branch and opened a pull request for you using background agents. Often, you will want to pull the branch down to your own computer, look at the change, make one or two edits, and push it back up.

## Before you start

You need:
- Github Desktop installed
- Cursor installed
- The repo downloaded on your computer
- A branch or pull request created by your background agent

If you have not downloaded the repo yet, open the repo in Github and select `Code --> Open with Github Desktop`.

![[CleanShot 2026-05-01 at 16.25.27.png]]

## What is happening?

A branch is a simply a separate version of the files in your project. The main branches controls what is deployed to users, and other branches are used to develop software. When a feature branch is ready, it can be merged into main.

Your background agent made changes on its own branch. You are going to:

1. Switch to that branch locally
2. Open the files on your computer
3. Make a small edit
4. Commit the edit
5. Push the branch back to Github

After you push, the pull request will update automatically.

## 1) Open the branch locally

Open Github Desktop. At the top, confirm you are in the correct repo.
![[CleanShot 2026-05-03 at 11.18.37.png]]
Click the branch dropdown. You should see the branch created by Cursor or your background agent.
![[CleanShot 2026-05-03 at 11.19.01.png]]

Select the branch. Github Desktop will pull the files for that branch onto your computer.
If Github Desktop asks whether you want to fetch or pull, click the option to get the latest changes.

## 2) Open the project in Cursor

From Github Desktop, click `Open in Cursor` or `Open in External Editor`.
![[CleanShot 2026-05-03 at 11.19.43.png]]

If you do not see Cursor, open Cursor manually and choose `File --> Open Folder`. Select the repo folder. You are now looking at the background agent's branch on your computer.

## 3) Review the change

Before editing, ask Cursor to explain the branch.

Use a prompt like:

`Explain the changes on this branch in plain English. Tell me what files changed, what the feature does, and what I should review before pushing anything.`

You can also ask:

`Review this branch for unnecessary changes, new components that should reuse existing components, missing analytics events, missing tests, and places where the data model may have been invented instead of reused.`

Note that AI models will often oversell the severity of issues. A friendly engineering review goes much further than continuing to ask the AI to find issues; it will likely find new problems every time you ask.
## 4) Make a small change

Make one small edit.

Good examples:

- Fix copy
- Tighten a label
- Add a missing empty state message
- Remove an unnecessary file
- Ask Cursor to reuse an existing component
- Add a missing screenshot placeholder in a docs change

Use a prompt like:

`Make the smallest possible change to address this feedback. Do not restructure unrelated files. Do not create new components unless there is no existing component to reuse.`

## 5) Check the changed files

Go back to Github Desktop. You should see your changed files listed on the left.

![[CleanShot 2026-05-03 at 11.25.06.png]]

Click through the files and review the diff.

Look for:

- Did only the expected files change?
- Did the AI create new files you do not understand?
- Did it change formatting across unrelated files?
- Does the change still match the original goal?

If something looks wrong, go back to Cursor and ask for help before committing.

## 6) Commit your change

In Github Desktop, write a short summary of what changed.
Examples:

- `Tighten empty state copy`
- `Reuse existing analytics card component`
- `Add missing verification note`
- `Fix background agent guide typo`

You can also ask cursor to commit for you. After, you can view your commit in your branch history.
![[CleanShot 2026-05-03 at 11.25.59.png]]

This commit only lives on your computer for now and needs to be pushed to Github.

## 7) Push your change

After committing, click `Push origin`. If you don't see Push Origin, you may need to push the branch first.
![[CleanShot 2026-05-03 at 11.26.31.png]]

This sends your local commit back to Github. If there is already a pull request open, the pull request will update automatically.

## 8) Check the pull request

Open the pull request in Github. You can access it easily from Github Desktop:
![[CleanShot 2026-05-03 at 11.27.26.png]]

You may have errors on your Pull Request; we'll cover that in 8 - Submitting Your Pull Request.

## Common problems

### I do not see the branch

Click `Fetch origin` in Github Desktop.

If you still do not see it, open the pull request in Github and check the branch name.

### Github Desktop says I have uncommitted changes

That means you have local edits that have not been saved as a commit.

If they are your edits, commit them.

If you do not know what they are, ask Cursor:

`Explain the uncommitted changes in this repo. Tell me whether they look related to my current task.`

### I am on the wrong branch

Check the branch name at the top of Github Desktop.

If you are unsure, do not commit yet.

Ask Cursor:

`What branch am I on, and how does it relate to the open pull request?`

### The app does not run locally

That is common.

Some products need Docker, databases, secrets, or many services.

If the app does not run locally, you can still review the diff, edit files, and push changes.

Just be honest in the PR:

`I reviewed the diff and made a copy change locally. I could not run the app locally because setup requires [reason].`

### I pushed but the PR did not update

Make sure you pushed to the same branch used by the pull request.

In Github Desktop, check the branch name.

In Github, check the pull request branch name.

They should match.

## Optional: Terminal commands

If you are comfortable with the terminal, the same workflow is:

```bash
git fetch
git checkout branch-name
git pull
```

Make your edits, then:

```bash
git status
git add .
git commit -m "Short description of change"
git push
```

If you are new to Github, use Github Desktop instead.
