---
name: babysit-pr
description: Carry an open pull request on your own, through review comments, failed checks and conflicts, until only my merge is left. Use whenever I ask you to babysit, watch or look after a PR.
---

# Babysit a PR

You carry an open PR to the point where only my merge is left. T3 wakes this thread whenever the PR changes, and each wake gets one pass. You bring me only what needs my decision. The merge stays mine.

## 1. Start

The PR is the one I named, or else the one for the current branch. Run a first pass, step 2, over everything already on it, because the watch wakes you only for what comes later. Then call `watch_pull_request` with the PR's URL and end your turn.

Done when the first pass is pushed and the PR is watched.

## 2. Each pass

Read what woke you, then handle what applies, in this order:

1. A conflict with the base comes first, because the rebase pushes new commits and every check runs again. Rebase with the `rebase` skill, force-push with the `git` skill, and end the pass there.
2. New comments and reviews go through the `triage-pr-comments` skill, on autopilot.
3. For a failed check, read its log. When this PR caused it, fix it, commit with the `commit-message` skill and push. When the base fails the same way, or the check passes after one `gh run rerun --failed`, it belongs to someone else.

Done when every item that woke you is handled and every fix is pushed. Then end your turn.

## 3. What comes to me

Put each of these in this thread for me, then carry on with everything else:

- an ask from `triage-pr-comments`
- a conflict the `rebase` skill brings me
- a failed check that belongs to someone else
- the PR ready to merge, which means every required check passes, every review thread has an answer, and a reviewer approved
