
A pull request is a request to merge one branch into another branch. Most teams merge feature branches into `main`.

You can think of it as saying:
`Here are my changes. Please review them before they become part of the product.`

## Before you start

You need:

- A branch with your changes
- Your branch pushed to Github
- A pull request open in Github
- Github Desktop installed
- Cursor or another editor installed

![](<screenshots/CleanShot 2026-05-03 at 11.29.38.png>)

## 1) Review your own diff first

Open the pull request in Github and click the `Files changed` tab.

![](<screenshots/CleanShot 2026-05-03 at 11.29.50.png>)

Before asking anyone else to review, look through the changed files yourself.

Check:

- Are the changed files expected?
- Did the AI touch unrelated files?
- Did it create new components when existing ones could be used?
- Did it invent a new data model instead of using an existing one?
- Did it add analytics events if the product behavior changed?
- Did it remove tests or disable checks?
- Are there TODOs, console logs, mock data, or debug code?

If anything looks wrong, go back to Cursor and fix it before moving on.

Use a prompt like:

`Review this PR before I ask for human review. Look for unrelated changes, missing tests, missing analytics, new components that should reuse existing components, invented data models, and anything that would make this PR hard to merge.`

## 2) Run tests locally

If the repo can run on your computer, run the relevant checks before relying on CI. This catches simple issues before reviewers spend time on the PR.

Common commands are:

```bash
npm run lint
npm run typecheck
npm test
npm run build
```

Your repo may use different commands.

Ask Cursor:

`Look at package.json and tell me the relevant commands to for CI/CD before submitting this PR. Then run them and tell me if anything fails.`

![](<screenshots/CleanShot 2026-05-03 at 11.31.52.png>)


If the app does not run locally, do not pretend it does. Explain what you could and could not verify.
## 3) Push your latest commits

If you made local edits, make sure they are committed and pushed before you ask for review.

In Github Desktop:

1. Review the changed files
2. Write a short commit message
3. Click `Commit to [branch name]`
4. Click `Push origin`

![](<screenshots/Pasted image 20260503113259.png>)


After pushing, refresh the pull request in Github. Your latest commit should appear in the PR.

## 4) Wait for CI/CD

CI/CD is the automated system that checks your PR. Depending on the repo, it may run lint, typecheck, tests, builds, security checks, or preview deployments.

On Github, these checks usually appear near the bottom of the PR.

![](<screenshots/CleanShot 2026-05-03 at 11.33.21.png>)

If checks pass, you can move to review. If checks fail, click into the failing check and read the error.

Ask Cursor:

`This CI check failed. Read the error and explain what it means in plain English. Then suggest the smallest fix.`

## 5) Update your branch from main

Before merging, your branch should be up to date with `main`. This matters because other people may have merged changes while you were working.

If your branch is out of date, Github may show a message like:

- `This branch is out-of-date with the base branch`
- `Update branch`
- `This branch has conflicts that must be resolved`

The simplest option is usually to click `Update branch` in Github.

![](<screenshots/CleanShot 2026-05-02 at 22.48.19.png>)

This brings the latest changes from `main` into your branch. After updating, CI/CD may run again, so wait for the checks to finish.

## 6) Fix conflicts

A conflict happens when your branch and `main` changed the same part of the same file. Github will not know which version to keep.

If you see a conflict warning, do not guess.

Ask Cursor:

`This branch has merge conflicts. Explain which files conflict and what changed on each side. Help me resolve the conflict while preserving the intended behavior.`

After resolving conflicts:

1. Review the files carefully
2. Run the relevant checks again
3. Commit the conflict fix
4. Push the branch
5. Wait for CI/CD again

## 7) Make the PR easy to review

Before asking for review, write a clear PR description. Keep it short, but make sure a reviewer can understand what happened.

Include:

- What changed
- Why it changed
- How you verified it
- Anything you could not verify
- Any risky areas reviewers should inspect
- A screenshot or recording of any frontend changes

Example:

```md
## Summary

Adds an empty state to the activity dashboard when no runs are available.

## Verification

- Ran npm run lint
- Ran npm run typecheck
- Manually reviewed the empty state copy

## Notes

I did not run the full app locally because local setup requires Docker.
```


## 8) Merge only when ready

Do not merge just because the button is green.

Before merging, confirm:

- The diff is small and understood
- CI/CD passes
- The branch is up to date with `main`
- Conflicts are resolved
- Required reviewers approved
- Analytics, docs, and support notes are handled if needed
- There is no unresolved risk in the PR description

For this course, stop here. You do not need to merge the PR.

## Common problems

### CI fails

Click into the failing check and read the error before changing anything. Do not make random fixes.

Ask Cursor:

`Explain this CI failure and identify the smallest change that is likely to fix it.`

### CI is still running

Some checks take several minutes. Wait for the important checks to finish before requesting review, unless you clearly say checks are still running.

### The branch is out of date

Click `Update branch` if Github offers it. If your team requires rebase, ask for help before force pushing.

### Merge conflicts appear

Do not resolve conflicts blindly. Ask Cursor to explain both sides of the conflict, then review the final file yourself.

### Tests pass locally but fail in CI

This can happen because CI may use a different environment, Node version, database, or secrets.

Open the failing CI log and ask Cursor to compare it to your local setup.

### The PR got too big

Split it.

Ask Cursor:

`This PR is too large. Help me split it into the smallest useful PR and a follow-up.`

Small PRs get reviewed faster and create less risk.

### Someone leaves comments

Do not treat comments as failure. Reply clearly, and if you make a change, push another commit.

If you disagree, explain why and ask a question.

## Optional: Terminal commands

If you are comfortable with the terminal:

```bash
git status
git pull
npm run lint
npm run typecheck
npm test
git push
```

To update from main with rebase:

```bash
git fetch origin
git checkout your-branch-name
git rebase origin/main
git push --force-with-lease
```

Only rebase if your team expects it.
