---
title: Languages
order: 413
description: How a quest, group or pass reads in each player's language, and the content keys.
icon: languages
---

A quest, group, pass or reward names its words as `@some.key`, a key of `messages/`, so one quest reads in every
language the server has. Text without a leading `@` is shown as written, for a server with one language.

| Shipped key | Holds |
|---|---|
| `group.<group>.name`, `.description` | The shipped groups |
| `quest.<quest>.name`, `.description` | The shipped quests |
| `pass.<pass>.name`, `.description`, `pass.tier.*` | The shipped pass |
| `feedback.*` | The four moments |
| `task.<type>`, `task-any.<type>` | A task with no description |
| `vanilla.material.*`, `vanilla.mob.*` | Block, item and mob names |

A pass tier line can pass a value into a message: `[message] @pass.tier.reached tier=1`.

How a player's language is chosen is in [Languages](../config/languages.md).

Related: [Messages](../config/messages.md)
