---
name: triage-findings
description: Judge a finding through five checks to a verdict and its evidence. Use whenever you decide what to do with a finding, from a PR comment, a reviewer, or a subagent's report.
---

# Triage findings

A finding is a claim that something in a change is wrong. Every finding is a hypothesis, whoever raised it, a person, a review bot or your own subagent. Test it through the funnel and act only on what survives. The change ships what it set out to do, with the real problems fixed and nothing added out of fear.

Start from the change's intent, two sentences written from its PR, its issue and its design. Each check reads the finding against that intent.

## The funnel

Each finding goes through five checks in order and leaves at the first one it fails.

| Check | Passes when | Often fails on |
|---|---|---|
| Current | the code the finding describes is still there | an outdated thread whose code moved or changed. Search the file for the symbol before failing it |
| True | the code does what the finding claims | a misread of the code, an API the installed version lacks, a case the callers already handle |
| Reachable | a real user of this product can trigger it today | two tabs editing one thing, users or organisations we don't have, input the types or schema rule out, scale we don't run at, consumers that don't exist |
| In scope | fixing it belongs to the change's intent. A defect the change introduced always does | "while you're at it", a refactor of nearby code, an option or abstraction for a future need, a problem in code the change didn't touch |
| Worth it | the failure costs more than the code that prevents it | a null check the types rule out, a catch that hides an error, a guard repeated at an inner layer, a style preference no project rule backs, a failure that already surfaces loudly before it costs anything |

A claim about behaviour is true only when a run shows it, such as a failing test or a script and its output. Facts about the product come from the project's `AGENTS.md`, its README and its docs. A fact none of them settles is mine to give.

## Verdicts

- A finding that fails current, true, reachable or worth it is a decline.
- One that fails in scope is a defer.
- One that turns on a fact only I know is an ask.
- One that passes all five is a fix.

When three or more findings attack variations of one mechanism, such as a third edge case of one guard, the mechanism itself becomes an ask instead of the next fix.

Every verdict carries its evidence:

```
verdict   fix, decline, defer or ask
reason    the check that decided it, with the evidence: path:line, a command and its output, or the fact only I know
remedy    for a fix, the smallest change at the layer that owns the cause
```

## Remedies

The remedy for a fix is yours, not the reviewer's. Fix the cause at the layer that owns it. A type that makes the bad state impossible beats a runtime check, and an error that stays loud beats a fallback that hides it.

Done when every finding has a verdict and the evidence behind it.
