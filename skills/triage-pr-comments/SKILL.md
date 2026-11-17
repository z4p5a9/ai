---
name: triage-pr-comments
description: Triage the review comments on a pull request. Use whenever you look at, address, or act on PR comments.
---

# Triage PR comments

Every comment on a PR is a finding to test, and the `triage-findings` skill holds the funnel that tests it. This skill collects the comments, sends them through the funnel, and acts on what survives.

## 1. Collect

Read the PR's title, description, linked issues, and diff, and write its intent in two sentences before you read any comment.

In the project's checkout, run `node <skill folder>/pr-comments.ts collect`, where `<skill folder>` is this skill's folder. It prints every comment on the current branch's PR, or on the PR you were given, that does not carry our 🚀 yet. When the PR had an earlier round of review, also run `node <skill folder>/pr-comments.ts history`. It prints every review with the commit it saw, and every thread with its replies, for the later-rounds checks in `triage-findings`. Turn the comments into findings:

```
finding
  claim     what is wrong, in one sentence, in your words
  where     path:line
  sources   every comment that raised it, by id
```

One review body often holds many findings, including folded nitpicks and "outside diff range" sections. One finding raised by several reviewers is one finding with several sources. A comment with nothing to test, such as a walkthrough summary, is a notice.

A finding about something we already met while building this PR and decided not to tackle is a decline that names that decision. It goes to no investigator.

Done when the intent is written and every collected comment is a source of a finding or a notice.

## 2. Investigate

Group findings that one agent should investigate together, because the code and context they need overlap. A finding that shares nothing with the others gets its own agent.

Send each group to its own subagent, all in parallel. Give each one the PR's intent, its findings with their full threads, and in a later round the output of `history`. Tell it to judge each finding with the `triage-findings` skill. Investigators read and run code, and leave the working tree as they found it. Each returns a verdict for every finding, in the shape `triage-findings` sets.

Done when every finding has a verdict with evidence behind it.

## 3. Report

Read every verdict and its evidence. You own each one. Where the evidence does not hold, decide the verdict yourself with `triage-findings`.

Show the user the asks and fixes in full, and the defers and declines one line each:

```
Ask
  path:line · reviewers · the claim
    what you need from the user, and the options
Fix
  path:line · reviewers · the claim
    the evidence, then the remedy
Defer   path:line · the claim · why it waits
Decline path:line · the claim · the check it failed
Notices: their count
```

Done when every finding and notice appears once.

## 4. Discuss

Go through the report with the user. They answer the asks, change any verdict they disagree with, and tell you to go ahead. Verdicts they don't mention stay as they are. If they want to leave something for later, leave it. It comes up again the next time you triage this PR.

On autopilot, when the user told you to handle PR comments without them, as a babysit skill does, there is no discussion. Act on every verdict except the asks, and leave those for the user.

Done when the user has answered every ask and told you to go ahead, or right away on autopilot. Then read `ACTING.md` in this skill's folder and follow it.
