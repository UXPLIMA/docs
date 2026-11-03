---
title: Group files
order: 502
description: The keys of groups/<id>.conf, at group, quest and task level.
icon: file-text
---

| Level | Keys |
|---|---|
| `group` | `name`, `description`, `mode`, `count`, `claim`, `sharing`, `rerolls`, `rule`, `reset`, `feedback` |
| `rarities` | A list: `name`, `weight`, `least`, `most` |
| `quests` | A list: `id`, `name`, `description`, `rarity`, `rule`, `tasks`, `rewards`, `feedback`, and for a catalogue `category`, `requires`, `cooldown` |
| A task | `id`, `type`, `amount`, `description`, `feedback`, `reverse`, and the options of its type |
| `rewards` | `set`, `milestones` |

`daily.conf` and `catalogue.conf` ship. What each key does is in [Groups and boards](../features/groups-and-boards.md),
[Resets](../features/resets.md), [Catalogues](../features/catalogues.md) and [Quests and tasks](../features/quests-and-tasks.md).

Related: [Task types](../features/task-types.md)
