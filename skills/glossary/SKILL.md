---
name: glossary
description: Write, edit, and review terms in a project's GLOSSARY.md.
disable-model-invocation: true
---

# Glossary

The glossary is the shared language between you and me on a project. Each term pins one concept to one word, so we both mean the same thing when we use it. A term says what the concept is and where it ends, and the code stays the source of truth for how it works. It is our internal language, not the words used in external copy, even when the two overlap.

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

A project without a glossary gets `GLOSSARY.md` at its root, and this section in its `AGENTS.md`.

```md
## Glossary

The project keeps a [glossary](./GLOSSARY.md) so we both speak the same language. Read it in full before you start work.
```

## Term

A term is a name, a definition, and the words to avoid in its place.

A term is expensive. Both of us carry it in every conversation, every definition that uses it binds to it, and every reread checks it. Be conservative, and add a term only when the concept earns one.

### What earns a term

A term is deep, the way a deep module is. A short word sits over a concept with real depth, and the implementation underneath it can grow and change while the term stays the same. A term names a concept of the domain, and the pieces of the implementation, such as a field, a flag, a file, a helper, or a step inside a process, keep the names the code gives them.

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

## 1. Read

Read GLOSSARY.md in full, and the code and docs where each concept shows up. For a review, read the code behind every term. Done when you can name each concept's neighbours in the glossary, what the code calls it, if anything, and where it ends.

## 2. Ask

Ask me where a concept's boundary isn't settled by the code or our conversation, such as whether storage means our code or the system it talks to. Ask too about a concept I named that you think doesn't earn a term, and say why. Send every question in one message. Done when every boundary is settled, or there were no questions to ask.

## 3. Write

Write the entries straight into GLOSSARY.md. Write a term after the terms its definition uses. A new term goes in the cluster of the terms it is defined with, a nested term directly under its parent, and an edited one stays where it is.

## 4. Reread

Reread the whole glossary, not only the entries just written. A review runs 1 and then starts here.

- Is every term a deep concept that still earns its place?
- Is every term one lowercase word or kebab-case compound, and every nested term under a parent in the glossary?
- Is every word in a definition plain or another term?
- Does any definition use a word another term avoids to mean that term's concept?
- Does every avoid list match the current terms?
- Does every rule sit under the term it constrains?
- Would every definition stay true if the code under it were rewritten?

Fix every finding in GLOSSARY.md. Done when every check passes, or a finding needs a call only I can make.

## 5. Report

Show me every new, changed, and removed entry in glossary format, with the old entry beside the new one, and every finding left open for me. Leave the commit to me. Done when I have seen every entry you touched.
