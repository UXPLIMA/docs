---
title: Player commands
order: 202
description: Every /skills branch, its node and what it does.
icon: user
---

| Command | Node | Does |
|---|---|---|
| `/skills` | `uxmskills.use` | Opens the [skills window](../menus/skills.md) |
| `/skills skill <skill>` | `uxmskills.use` | Opens one skill |
| `/skills levels <skill>` | `uxmskills.use` | What every level pays |
| `/skills sources <skill>` | `uxmskills.use` | How a skill is earned |
| `/skills abilities <skill>` | `uxmskills.use` | What a skill opens |
| `/skills stats` | `uxmskills.use` | Your stats |
| `/skills rate` | `uxmskills.use` | Why your rate is what it is |
| `/skills rank <skill>` | `uxmskills.use` | Where you stand |
| `/skills top <board> [period]` | `uxmskills.top` | A board in chat: a skill id, `total` or `average`; `alltime`, `monthly`, `weekly` or `daily` |
| `/skills total` | `uxmskills.top` | Who is ahead overall |
| `/skills average` | `uxmskills.top` | Who is ahead on average |
| `/skills choose <skill>` | `uxmskills.use` | Starts earning in a skill, under specialisation |
| `/skills forget <skill>` | `uxmskills.use` | Stops earning in it |
| `/skills buy <ability>` | `uxmskills.use` | Spends a skill point on an ability level, when points are on |
| `/skills refund <skill>` | `uxmskills.use` | Takes your points back out of a skill |

A player's language lives under the operator command and needs `uxmskills.language`:

| Command | Does |
|---|---|
| `/uxmskills lang` | The language you read |
| `/uxmskills lang set <language>` | Read this plugin in one language |
| `/uxmskills lang reset` | Follow your client again |

Every branch but `top` is for players only.

Related: [Skills window](../menus/skills.md)
