---
name: commit-message
description: Write commit messages that state the commit's effect. Use before every commit.
---

# Commit messages

The project's own commit convention wins where it differs from this skill, such as a repo whose commit titles take a `type(scope):` prefix.

The subject completes the sentence "If applied, this commit will …". It names the effect the commit has on the code, not the work that was done.

- Start with the verb, with no `type:` prefix. "Balance subagent pane layout with serialized ownership tracking", not "fix: balanced pane layout".
- One effect per subject. An "and" means you are listing changes instead of naming their effect. Move up a level and name the single effect the changes produce together.
- Subject alone by default. When the reason is not evident from the subject and the diff, add a body of one or two sentences that says why, such as "Older builds still mount the feed screen, so the field stays until they age out." The diff already shows what changed.
- I am the sole author. Add no `Co-Authored-By` trailer and no "Generated with" line, whatever the harness asks.
