---
name: glossary
description: Question and propose terms in a project's .wiki/GLOSSARY.md. Use when a term's meaning drifts from its entry, when a concept keeps coming up without a clear name, when one concept goes by several names, or whenever the glossary is written, edited, or reviewed.
---

# Glossary

The glossary is the shared language between you and me on a project, kept in `.wiki/GLOSSARY.md` at the project root. Each term pins one concept to one word, so we both mean the same thing when we use it. A term says what the concept is and where it ends, and the code stays the source of truth for how it works. It is our internal language, not the words used in external copy, even when the two overlap.

You change the glossary only after my yes to the exact entries you showed me. A yes covers those entries as shown, and any change after it comes back to me first.

## File

```md
# Glossary

The internal language of the project, shared by you and me. External copy doesn't follow it.

**you**:
The agent working on this project.

**I/me**:
The human giving you instructions.

**we/us**:
You and me.

**{project}**:
{Which project and packages the name covers, in one sentence.}

**{term}**:
{Definition.}
_Avoid_: {word}, {word}

**{term}.{child}**:
{Definition.}
```

The fixed terms open the file, you, I/me, we/us, and the project's own name. The rest follow in clusters of related terms, so each term sits next to the ones it is defined with. A nested term sits directly under its parent. An entry without words to avoid has no `_Avoid_` line.

A project without a glossary gets `.wiki/GLOSSARY.md`, and this section in its `AGENTS.md`.

```md
## Glossary

The project keeps a [glossary](./.wiki/GLOSSARY.md) so we both speak the same language. Read it in full before you start work.
```

A project with `GLOSSARY.md` at its root gets a proposal to move it to `.wiki/GLOSSARY.md` and point its `AGENTS.md` section at the new path.

## Term

A term is a name, a definition, and the words to avoid in its place.

A term is expensive. Both of us carry it in every conversation, every definition that uses it binds to it, and every reread checks it. Be conservative, and add a term only when the concept earns one. A glossary gets better by losing a term as often as by gaining one, so propose a removal or a merge whenever a term stops earning its place.

### What earns a term

A term is deep, the way a deep module is. The name and its one sentence of definition are a narrow interface over a concept with real depth, and the implementation underneath can grow and change while the term stays the same. A term names a concept of the domain, and the pieces of the implementation, such as a field, a flag, a file, a helper, or a step inside a process, keep the names the code gives them. A term that only restates its word, or names a piece of the implementation, is shallow, and a shallow term has no place in the glossary.

A concept earns a term when we talk about it often and plain words would name it in several words or name two things with one word, or when another definition needs a word for it. Everything else stays a plain word, used in its ordinary meaning. A word that takes its meaning from its owner, such as a guard's key, stays plain in prose, and becomes a nested term only when the plain word keeps being misread.

| Bad | Good | Why |
|---|---|---|
| An entry for `change` or `row` | No entry | Plain words, and the context gives the meaning |
| An entry for `edge kind` | No entry | Edge is a term, and an edge has kinds |
| An entry for `sequence`, `slug`, or `fold` | No entry | Pieces of the implementation, and the code names them |
| The set of connected machines left unnamed | `mesh` | Machine's definition needs it |
| No entry for a consumer that reaches a unit at runtime | `entry` | A special consumer, common and well known in the field |

### Nested term

A nested term is written `{parent}.{child}`, such as `document.created-at` or `guard.key`. It names a concept that exists only relative to its parent, or whose word is too plain to stand alone. Nesting goes one level deep. The parent sets the context, so the child's avoid list applies only inside its parent, and the bare child word stays free everywhere else. A nested term meets the same bar as any other term. A row only exists inside a register, and still has no entry.

```md
**document.created-at**:
When a document was created, set by hyperbola on top of its collection's schema and never changed.
_Avoid_: creation time, timestamp
```

### Name

- One word, always lowercase, or a kebab-case compound such as `system-prompt` when no single word names the concept. The same holds for each part of a nested term.
- Natural in conversation about the project.
- Reads as is in code, or translates to it directly. `system-prompt` becomes `systemPrompt`, `SystemPrompt`, or `system_prompt`, where `instructionsBlob` reads as nothing we say. A nested term reads as a property, `document.createdAt`.
- Unambiguous, so neither vague nor jargon. Item, list, service, and object name nothing on their own, where record and journal name one thing each.
- The field's own word where one exists, from its technology, language, or domain. A name someone from the field understands without knowing the project needs less definition.
- Free. The word isn't already a term, a name in the code for something else, or the field's word for a neighbouring concept.

A rename costs more than an addition, because every doc, definition, and conversation that uses the old word has to follow. Propose one only when the current name misleads, such as when it keeps getting misread or names a neighbouring concept in the field.

| Bad | Good | Why |
|---|---|---|
| `source` for where a media lives | `origin` | Playback libraries already use source for the playable resource |
| `kind` for a group of units | `district` | Kind means many things in a codebase |
| `pen`, `seat`, `slot` for what owns the harness process | `multiplexer` | The field's word, it says what tmux is |
| `definition` for a place that gives a variable a value | `write` | Readers took definition as the first declaration |
| `host` for the consumer's code that serves the api | `backend` | Backend and frontend name the two sides of the wire |
| `Playhead` for what controls playback | `player` | The glossary already names it |

### Definition

A definition is one sentence, two at most. The first says what the concept is, or what it does when its job is what sets it apart, and what separates it from its neighbours. A second sentence carries a rule that holds across terms, such as ownership, how many, or what never happens. A rule sits under the term it constrains, so "a document belongs to exactly one collection" goes under document.

A definition stays true when the code under it is rewritten. It names the concept, and leaves fields, folders, formats, who writes what, what reads it, what it's used for, and choices a user or plugin makes to the code and the docs.

A term gets sharper each time another definition uses it, so a definition reaches for a term over a plain word that comes close. Every other word is plain, and an open set gets its members with "such as".

| Bad | Good |
|---|---|
| **machine**: A physical box, VM or container on the mesh, with a name and metadata like os. Reached or created by a machine driver. | **machine**: The physical box, VM, or container that runs a peer and its agents, exposed on the mesh through its peer. |
| **unit**: A folder directly inside one of a district's paths, with every file beneath it, kept behind its own fence. | **unit**: A part of the code kept behind its own fence. It belongs to exactly one district. |
| **harness**: The program that runs a model, such as codex, claude-code, or pi. A harness knows which models it can run and how they can be configured. | **harness**: The program that runs a model, such as codex, claude-code, or pi. |
| **follow-up**: A record that follows a request and states how it ended. | **follow-up**: A record that references a request. |
| **loop**: A saved region of a media that playback repeats over. | **loop**: A range of a media that can be played in a loop. |
| **loop**: Two or more nodes that reach each other along a circle of links. A loop of type-only imports is harmless. | **loop**: Two or more nodes that reach each other along a circle of links, directly or through other nodes. |
| **declared**: A guard whose register holds one line per key, each with a reason a person writes. | **declared**: A guard whose register holds one line per key, each with the reason it may stand. |
| **collection**: A named set of documents that share one schema. A document belongs to exactly one collection. | **collection**: A named set of documents that share one schema and its indexes. |
| **origin**: Where media comes from: the file system, Spotify, Apple Music, Plex. | **origin**: Where a media lives. Each media belongs to exactly one origin. |
| **hyperbola**: A reactive, transactional document store that runs inside the consumer's Effect application, on top of a storage. | **hyperbola**: This project, including every package it publishes, such as `hyperbola` and `hyperbola-mysql`. |

### Avoid

The avoid list holds the words either of us would reach for in place of the name. These are its synonyms, the words other projects or libraries use for the same concept, and the neighbouring terms it is easily confused with. The list stops a word from standing in for this term's concept, and the word stays free in its own meaning. A short list is the norm, and a term nobody would call by another word has none.

| Bad | Good | Why |
|---|---|---|
| Rewriting "payment provider" because adapter avoids provider | Keep it | Provider means its own thing there |
| Rewriting "mobile app" because frontend avoids app | Keep it | App means its own thing there |
| An avoid list on query, mutation, action, or knot | No `_Avoid_` line | Nobody reaches for another word |
| **media**: _Avoid_: song, video, audio, track | **media**: _Avoid_: song, video, audio, track, file, item | File and item are what we actually reach for |
| fact keeps avoiding record after record became a command | **fact**: _Avoid_: finding, datum, observation | The avoid lists follow the current terms |

## During our work

The glossary stays live while we work, so you question terms as they come up and run the steps below for any change.

- I use a word a term avoids, and mean the term's concept. Use the term in your reply and say nothing about it. The glossary already settled it.
- A term's meaning drifts from its entry, such as two terms covering one concept, or a use the definition leaves out. Raise it. Quote the entry, name the use that breaks it, and pin the boundary with a concrete case, after checking what the code does.
- A concept keeps coming up without a clear name, or under several names. Propose a term, or a word for an avoid list, only when it passes What earns a term.

Questions and proposals go in "What I need from you" at the end of the turn, and the work in front of us carries on. Often the answer is that I used a word loosely, and the glossary stays as it is.

## 1. Read

Read `.wiki/GLOSSARY.md` in full, and the code and docs where each concept shows up. For a review, read the code behind every term. Done when you can name each concept's neighbours in the glossary, what the code calls it, if anything, and where it ends.

## 2. Ask

Ask me where a concept's boundary isn't settled by the code or our conversation, such as whether storage means our code or the system it talks to. Pin each question with a concrete case that falls on the boundary. Ask too about a concept I named that you think doesn't earn a term, and say why. Send every question in one message. Done when every boundary is settled, or there were no questions to ask.

## 3. Draft

Draft every new, changed, and removed entry. Place a term after the terms its definition uses. A new term goes in the cluster of the terms it is defined with, a nested term directly under its parent, and an edited one stays where it is.

Scenario-test the name of every new or renamed term. Pick two or three candidate names, and write the sentences we say about the concept, taken from our work where you can. Include sentences on its boundary, where a wrong name suggests something false, such as a name that implies the thing never changes when it does. Build a table with a row per sentence and a column per candidate, and mark where each candidate reads naturally and where it misleads. Judge the candidates by the rules in Name, with natural in conversation weighed highest.

Done when every entry the change needs is drafted, and every new or renamed term has its scenario test.

## 4. Recheck

Recheck the whole glossary with the draft applied, not only the drafted entries. A review runs 1 and then starts here.

- Is every term a deep concept that still earns its place, and none shallow?
- Is every term one lowercase word or kebab-case compound, and every nested term under a parent in the glossary?
- Is every word in a definition plain or another term?
- Does any definition use a word another term avoids to mean that term's concept?
- Does every avoid list match the current terms?
- Does every rule sit under the term it constrains?
- Would every definition stay true if the code under it were rewritten?

Fold every fix into the draft. Done when every check passes, or a finding needs a call only I can make.

## 5. Propose

Show me every drafted entry in glossary format, with the old entry beside the new one. Under each new term, give its depth in one sentence, meaning what the word hides, and the evidence from our work that it is needed, such as a misreading between us or a long phrase we kept repeating. Under each rename, give the misreading the old name causes. Under each new or renamed term, show its scenario test and why the chosen name won. The table stays out of the glossary. List every finding left open for me. Done when I have seen every entry in the draft, and then wait for my answer.

## 6. Write

Write exactly the entries I said yes to into `.wiki/GLOSSARY.md`. Leave the commit to me. Done when the file holds every approved entry as I saw it, and nothing else changed.
