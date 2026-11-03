---
title: Welcome
order: 201
description: What uxmLang does, who it is for, and where to read next.
icon: hand
---

Every UXM plugin speaks seven languages and asks the same question for each player: which one? Without uxmLang each
plugin answers it alone, and a player who picks Turkish in one plugin still reads another in English. With uxmLang
there is one answer. A player picks a language once, with `/lang` or in its window, and every UXM plugin on the
server, and on every server of a network, reads it.

## What it does

| Part | What it gives you |
|---|---|
| One choice | The language service uxmLib reads, so every UXM plugin draws a player's lines in the language chosen here |
| A window | `/lang` opens one flag per language; a click reads the server in it |
| Following the client | A player who chose nothing reads the language their client is set to |
| Forcing one | A server with one audience writes its language once, and everybody reads it |
| A network | The choice is a database row, read while the player logs in, so the next server opens in the right language |
| Instant changes | With Redis, a change reaches the server a player is on at once |
| Effects | What choosing does is a list of action lines: a message, a sound, a title |

uxmLib is inside the jar, so the plugin needs nothing else on the server.

## Read next

- [Installation](installation.md): the jar and the files.
- [Concepts](concepts.md): how a language is decided.
- [Your first setup](first-setup.md): three common servers.

Related: [Commands](../commands/overview.md), [Permissions](../permissions/reference.md)
