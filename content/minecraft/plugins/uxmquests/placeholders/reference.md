---
title: Reference
order: 701
description: What uxmQuests reads from PlaceholderAPI, and the tokens it fills itself.
icon: braces
---

uxmQuests registers no PlaceholderAPI placeholders. It reads them:

| Where | Reads |
|---|---|
| A `placeholder` task | Any PlaceholderAPI value, compared with `operator` and `value`; see [Task types](../features/task-types.md#checked-types) |
| Reward and feedback lines | PlaceholderAPI is applied after the plugin's own tokens |
| A condition in a line | `{if=%mcmmo_level_mining% >= 50}`; mcMMO levels are read from mcMMO itself |

## Its own tokens

| Where | Tokens |
|---|---|
| Reward lines | `{player}`, `<player>`, `%player%`, `%player_name%`, `{50-200}` spans |
| Feedback lines | `<quest>`, `<group>`, `<done>`, `<needed>`, `<percent>` |
| Messages | `<quest>`, `<group>`, `<done>`, `<needed>`, `<percent>`, `<pass>`, `<tier>`, `<tiers>`, `<xp>`, `<time>` |
| Windows | `%group%`, `%quest%`, `%done%`, `%phase%` and the rest on each [window page](../menus/overview.md) |

None of these is a PlaceholderAPI placeholder; they are filled inside the plugin's own lines.

Related: [Rewards and milestones](../features/rewards.md)
