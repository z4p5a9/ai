---
name: adr
description: Propose, question, and retire a project's ADRs in .wiki/adr/. Use when we make or reject a hard-to-reverse choice the code won't show, when a plan or the code contradicts an ADR, when an ADR stops holding, or whenever an ADR is written, edited, or reviewed.
---

# ADR

An ADR records a decision we made on a project that the code can't show, kept in `.wiki/adr/` at the project root. It states the decision and why we made it, so neither of us undoes it by accident or argues it again without cause. The code stays the source of truth for how the decision is carried out.

You change `.wiki/adr/` only after my yes to the exact ADRs you showed me. A yes covers those ADRs as shown, and any change after it comes back to me first.

## File

Each ADR is one file, `.wiki/adr/{number}-{slug}.md`, such as `0003-records-are-tombstoned.md`, and we refer to it as ADR-0003. The slug states the decision, like the title, so listing the folder lists the decisions. A new ADR takes the next number after the highest ever used, in the folder or in its git history, so a number never comes back after its ADR is deleted.

```md
# {The decision, stated as a fact that holds}

{The context, the decision, and why, in one to three sentences.}

_Rejected_: {alternative}, because {reason}.
_Revisit when_: {the condition under which the decision stops holding}.
```

The title is the decision itself, such as "Records are tombstoned, never deleted", where a topic such as "Deletion strategy" says nothing. `_Rejected_` holds an alternative only when someone would otherwise propose it again, and an ADR without one has no `_Rejected_` line. Every ADR has a `_Revisit when_` line, so either of us can check it against the code and the project and see whether the decision still holds.

An ADR speaks the project's glossary. Every word in it is plain or a glossary term, and a concept that needs a new term goes through the glossary skill.

A project without ADRs gets this section in its `AGENTS.md` with its first ADR.

```md
## ADRs

The project records its hard-to-reverse decisions as [ADRs](./.wiki/adr/). Before you start work, list them to know which exist, and read every ADR your work touches.
```

## What earns an ADR

An ADR is expensive. Every task starts by knowing which ADRs exist, each one holds back every change that touches it, and each goes stale when the project moves under it. Hold the bar high. The best ADR is the one we found a way not to need, and every ADR we remove is a win.

A decision earns an ADR only when all four hold:

- Hard to reverse. Undoing it means a migration, a rewrite across many places, or breaking something outside the repo.
- Hidden. A competent reader of the code would not guess it, or would "fix" it.
- Chosen. A real alternative existed, or a real constraint ruled one out.
- Deep. It covers a wide area, the way a deep module does, and its title and paragraph are a narrow interface over it. It stays true when the code under it is rewritten. A decision about one function, one file, or one library call is shallow, and the code carries it.

### Before an ADR

Before you propose an ADR, find a way to make it unnecessary. Try each in order, and stop at the first that works.

1. Make it reversible. A decision that is cheap to undo needs no record, because we just undo it.
2. Make the code carry it. A name, a type, a test, a lint rule, or the shape of the code states the decision where a reader meets it. A type, a test, or a lint rule also fails the build when someone breaks it.
3. Write the ADR, only when neither works, such as for a decision that spans the codebase with no single place in the code to live, or a constraint from outside the repo.

| Decision | Home | Why |
|---|---|---|
| zod for validation | The code | Cheap to swap, and the imports show it |
| Retry three times | A named constant | Local, and cheap to change |
| Money is stored as integer cents | A `Cents` type | The type carries it everywhere money goes |
| Domain code never imports the database client | A lint rule | The rule states it and fails the build when broken |
| Records are tombstoned, never deleted, because peers must sync deletions | ADR | Spans storage and sync, the data makes it hard to undo, and it holds whatever the implementation |
| No AWS, because of a compliance contract | ADR | No code can show a contract |

### Retiring

An ADR retires when it stops holding, when its `_Revisit when_` condition comes true, or when the code starts carrying it. A retired ADR is deleted, and git keeps its history. A changed decision is a new ADR with a new number, and the old one is deleted, so ADR-0003 always means the decision it was written for. A rewording that keeps the decision keeps the number.

## During our work

ADRs stay live while we work, so you watch for decisions as they happen, in our conversation, in a plan, in code you write, and in code you read. Run the steps below for any change.

- We make or reject a choice that passes What earns an ADR, and Before an ADR finds no way out. Propose an ADR.
- A plan, a change, or something I say contradicts an ADR. Quote the ADR and name the contradiction. Either the ADR no longer holds and we retire it, or the change gives way. Argue against an ADR only with a reason it didn't weigh.
- The code or the project no longer matches an ADR, its `_Revisit when_` condition came true, or the code now carries it. Propose retiring it.

Proposals go in "What I need from you" at the end of the turn, and the work in front of us carries on. A contradiction that blocks the work in front of us goes first.

## 1. Read

Read every ADR in `.wiki/adr/`, and the code each one touches. For a review, check every ADR against the code. Done when you can say, for each ADR, where the code follows it, whether its `_Revisit when_` condition has come true, and whether the code now carries it.

## 2. Ask

Ask me what the code and our conversation can't settle, such as a constraint from outside the repo, whether a choice was deliberate, or whether a rejected alternative is out for good. Pin each question with a concrete case. Send every question in one message. Done when every question is settled, or there were none.

## 3. Draft

Draft every new, changed, and retired ADR. Under each new or changed ADR, note each step of Before an ADR you tried and why it failed. Done when every ADR the change needs is drafted, and each new or changed one has its note.

## 4. Recheck

Recheck every ADR with the draft applied, not only the drafted ones. A review runs 1 and then starts here.

- Does every ADR pass all four tests of What earns an ADR?
- Could a step of Before an ADR remove any ADR?
- Does every ADR still hold, with its `_Revisit when_` condition still false?
- Is every title the decision itself, stated as a fact?
- Would every ADR stay true if the code under it were rewritten?
- Do any two ADRs overlap or contradict each other?
- Is every word plain or a glossary term?

Fold every fix into the draft. Done when every check passes, or a finding needs a call only I can make.

## 5. Propose

Show me every drafted ADR in full, with the old text beside the new for a change, and the reason for each retirement. Under each new or changed ADR, give the evidence from our work that it is needed, and its note from Draft. The note stays out of the ADR. List every finding left open for me. Done when I have seen every ADR in the draft, and then wait for my answer.

## 6. Write

Write exactly the ADRs I said yes to into `.wiki/adr/`, and delete the ones I agreed to retire. Add the ADRs section to `AGENTS.md` if the project has none. Leave the commit to me. Done when the folder holds every approved ADR as I saw it, and nothing else changed.
