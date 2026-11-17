---
name: review-code
description: Review a PR, a branch or uncommitted changes for the breaks that matter, against what the change set out to do. Use whenever you are asked to review code, mine or a teammate's.
---

# Review code

You review a change so its author can fix what matters before it merges. You read and run the code, and leave it unchanged. The change's intent is the contract. A finding shows the change breaking it, and the review ends when nothing in the contract is broken.

## 1. Understand

Read the PR description, its linked issues, its design when one exists, and the branch's commits. With no PR, take the intent from our conversation, or ask me for it. Write down:

- the goal of the change, in two sentences
- every behaviour the description or the design promises
- every limit they accept

A finding breaks a promised behaviour, a project rule, or something that worked before the change. Project rules sit in the `AGENTS.md` and `CLAUDE.md` files on the path to the changed code, and other files may hold them too. Output that is worse than ideal but inside an accepted limit is a note, and a note holds nothing back.

Done when the goal, the behaviours and the limits are written.

## 2. Fan out

Start reviewers with `delegate_task`, all in parallel, each with `role: "review"`, the goal, behaviours and limits from step 1, and one share of the work:

- every change against the base, with `git diff <base>...HEAD`, or `git diff HEAD` for uncommitted changes
- every seam and branch those changes touch, for gaps, edge cases, abstractions the change does not need, and simpler solutions
- every promised behaviour, for the code and the test that carry it

Write each check in a reviewer's brief as the behaviour the code must keep, such as "a removed member is no longer on the channel after the job runs", and check access rules through the app's own tests. OpenAI's filter stops a Sol session whose brief reads like an attack.

Reviewers read and run code, leave the working tree as they found it, and leave lint and the full suite to CI. Each finding comes with the run that shows it, such as a failing test or a script and its output. A finding belongs in this review only when it attaches to a line the diff changed.

Done when every reviewer has reported.

## 3. Filter

Group related findings, and judge each one with the `triage-findings` skill. Keep the fixes and the asks, ordered by severity, and drop the rest. On a PR, read its existing comments, and drop a finding one of them already raised.

Done when every finding left is a fix or an ask.

## 4. Report

Show me each fix and each ask in full, then the count of dropped findings:

```
path:line · the claim
  what it breaks, then the evidence
```

When nothing is left, say the change passes at its commit.

Done when every finding left appears once. Stop here unless I asked you to post the review.

## 5. Post

This step is for a PR only. Write one comment per finding, attached to its file and line, with the `alkis-github-voice` skill, for a reader new to the codebase. Submit every comment in one review with the event `COMMENT`:

```
gh api repos/<owner>/<repo>/pulls/<number>/reviews --input review.json
```

`review.json` holds `event`, `body` and `comments`, each comment with its `path`, `line`, `side` and `body`. Post nothing when no finding is left.

Done when the review is posted and I have its link.

## Later rounds

When a new commit lands on a PR you reviewed, review the diff since the last commit you reviewed, from step 1, together with your findings that are still open. Run `node <skills folder>/triage-pr-comments/pr-comments.ts history <pr>`, where `<skills folder>` holds this skill's folder, for the earlier rounds, the commit each one saw, and the author's replies, so `triage-findings` can run its later-rounds checks. Close each open finding that its fix or the author's reply settles.

Stop and bring me the open findings, with the cases that reproduce them, when a round breaks again what an earlier round broke, or when a third round ends with open findings. Either means the fix or the contract is wrong.
