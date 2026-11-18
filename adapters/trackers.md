# Issue tracker adapter

This page maps the tracker operations that the Kit uses to commands for each supported tracker.
The default tracker is GitHub Issues.

Last checked: 2026-11-19, against GitHub CLI 2.103.3.

## Operations

| Operation | GitHub Issues with the `gh` CLI |
| --------- | ------------------------------- |
| Create a label | `gh label create <name> --description "<text>" --color <hex>` |
| Create an issue | `gh issue create --title "<title>" --body-file <file> --label <label>` |
| Add a label | `gh issue edit <number> --add-label <label>` |
| Read an issue | `gh issue view <number> --json title,body,labels,state,comments` |
| Comment on an issue | `gh issue comment <number> --body-file <file>` |
| Close an issue | Comment first, then `gh issue close <number>` |
| Create a sub-issue | `gh issue create --parent <parent> --title "<title>" --body-file <file>` |
| Add a Blocking edge | `gh issue create --blocked-by <number>,<number> ...`, or `gh issue edit <number> --add-blocked-by <blocker>` |
| Count open blockers | `gh issue view <number> --json blockedBy --jq '[.blockedBy.nodes[] \| select(.state == "OPEN")] \| length'` |
| List sub-issues | `gh issue view <parent> --json subIssues --jq '.subIssues.nodes[] \| {number, title, state}'` |
| Claim a decision ticket | `gh issue edit <number> --add-assignee @me` |
| Search issues | `gh issue list --label <label> --state open --json number,title` |
| Link a pull request to an issue | `Closes #<number>` in the pull request body |
| Open a pull request | `gh pr create --base <base> --head <branch> --title "<title>" --body-file <file>` |
| Merge a pull request | `gh pr merge <number> --squash --delete-branch` |
| Watch CI | `gh run watch <run id> --interval 60 --exit-status` |

## Rules

- Write bodies to a file and pass `--body-file`, so that the shell does not change the text.
- Read with `--json` and only the fields you need.
- Watch CI at 60 seconds or slower.
  All sessions on one account share one API budget.
- Writing an `@name` mention notifies that person.
  Write a bare `@name` only for a real account that you mean to notify.

## Adding a tracker

Add a column with the same operations.
When the tracker has no labels, map each Kit label to the tracker's nearest field, and record the map in the Project's `docs/agents/issue-tracker.md`.
