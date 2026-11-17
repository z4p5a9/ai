---
name: git
description: Push a branch, shape its commits and force-push it safely. Use before you push a branch or rewrite its commits.
---

# Git

## Push

Push your branch by name, as `git push origin HEAD:refs/heads/<branch>`, adding `-u` the first time. A branch started from `origin/main` tracks main, so a bare `git push` can fail or push to main, depending on `push.default`.

## Shape the commits

Each commit is one step of the change that a reviewer can read alone. Before the PR opens, fold every commit that only fixes an earlier one, such as a lint fix, into the commit it fixes. Commit it with `git commit --fixup <commit>`, then fold with `GIT_SEQUENCE_EDITOR=: git rebase -i --autosquash <base>`, which needs no editor.

After the PR opens, add new commits only, so each review reply can link to its fix. A rebase onto the base is the one rewrite after that point.

## Force-push

A rewrite of a pushed branch goes out with a force-push. First run `git fetch origin <branch>` and `git log --oneline <tip before the rewrite>..origin/<branch>`. A commit listed there was pushed by someone else, such as a GitHub suggestion a reviewer applied, and the force-push would drop it. Show it to me and stop.

Otherwise push with `git push --force-with-lease --force-if-includes origin HEAD:refs/heads/<branch>`. The lease alone passes after a background fetch. `--force-if-includes` refuses unless your branch once held the remote tip.
