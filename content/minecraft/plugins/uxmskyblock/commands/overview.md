---
title: Overview
order: 201
description: The two roots, help, renaming words, and how permissions and island roles work together.
icon: list
---

## The roots

| Root | Alias | Who |
|---|---|---|
| `/island` | `/is` | Every player; `/is` with no word opens the [island menu](../menus/island.md#island-menu), or the [start window](../menus/island.md#start-an-island) for a player with no island |
| `/isadmin` | | Operators: the verbs of the game modes, loot and leaderboard displays, each behind its own node |

`/is help [page]` lists the words the reader may use, ten to a page.

## Who may use a word

Two things decide what a player can do:

| Question | Decided by |
|---|---|
| May they type the word at all? | A permission node, for the words on these pages that name one. A word with no node is open to everyone. |
| May they do it on this island? | Their island role. `/is kick` needs the node `uxmskyblock.island.kick` and a role that holds the Kick members permission. |

A word a player may not use stays out of their command list and tab completion. Typed anyway, it answers with the
no-permission line in their language. See [Permissions](../permissions/reference.md) and
[Roles](../features/roles.md).

## Renaming words

Every word is in [commands.conf](../config/commands-conf.md): give it a new name, add aliases, add words for the
readers of one language, or turn it off. A change needs a restart. A name another word already answers to is
refused with a warning in the console.

## Arguments

| Written | Means |
|---|---|
| `<player>` | A player name; most commands complete online players |
| `[word]` | Optional |
| `a \| b` | One of these words |
