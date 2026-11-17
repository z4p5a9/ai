---
name: rebase
description: Rebase a branch onto its base and resolve its conflicts by what each side set out to do. Use whenever a branch needs rebasing, such as a PR that conflicts with its base or has fallen behind it.
---

# Rebase

You rebase the current branch onto its base and resolve every conflict yourself, by what each side set out to do. You bring me only a conflict that needs my decision.

## 1. Prepare

The base is the branch named in the arguments, or else the PR's base branch, or else the branch `origin/HEAD` points to. Run `git fetch origin <base>`, so the rebase lands on the base as it is now. Stop and tell me if tracked files have uncommitted changes, or if the current branch is the base.

Create a backup branch at the current commit, named `backup/<branch>-<YYYYMMDD-HHMM>`. Done when the backup and the branch point at the same commit.

## 2. Rebase

Run `git rebase origin/<base>`, and continue after each resolution with `GIT_EDITOR=true git rebase --continue`, which keeps every commit message as it is.

A rebase swaps the sides git names. `ours` and `HEAD` are the base plus the commits replayed so far, and `theirs` is the branch commit being replayed. Call the sides by their branch names whenever you report.

At each conflict, first learn what each side set out to do. Read the base commits that touched the conflicted lines, the message and whole diff of the branch commit being replayed, and the PRs and issues behind both. Then resolve it so the result does what both sides set out to do, writing code neither side had when both goals need it. When the goals contradict, such as both sides changing one behaviour in different ways, the commits, PRs and issues usually say which change is newer or which one is the point of its side. That settles it.

Stop and bring me a conflict only when the goals contradict and nothing you can read settles which one wins. Say what each side wants and where that comes from, and propose a resolution with its reason. Apply it once I answer.

Done when `git status` shows no rebase in progress.

## 3. Check

Run the project's checks, the ones its `AGENTS.md` names, or else its test command. Fix each failure a resolution caused, and fold the fix into the commit that holds the resolution, with the fix-up steps of the `git` skill. A failure the base has too is not the rebase's, so report it and leave it.

Then read `git range-diff origin/<base> <backup> HEAD`, and confirm each commit still does what its message says.

Done when the checks pass or every failure left also fails on the base, and every commit still matches its message.

## 4. Report

Tell me the backup branch, and that `git reset --hard <backup>` returns the branch to where it was. List every conflict you resolved, one line each, with the file and what you kept from each side, and for contradicting goals, why one side won. Give the check results. End with `git range-diff origin/<base> <backup> HEAD`, the command that shows how each commit changed through the rebase.

Done when the report is out.
