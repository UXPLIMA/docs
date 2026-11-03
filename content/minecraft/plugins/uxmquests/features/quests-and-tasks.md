---
title: Quests and tasks
order: 404
description: A quest, its tasks, amounts and spans, filters and draws, and how names are found.
icon: list-checks
---

```hocon
{
  id          = mine-stone
  name        = "@quest.mine-stone.name"
  description = "@quest.mine-stone.description"
  rarity      = common
  tasks = [
    { id = dig, type = block-break, block = ["STONE", "DEEPSLATE"], amount = "64-128", reverse = true }
  ]
  rewards = [ "[message] @quest.mine-stone.reward", "[console] give {player} bread 4" ]
}
```

| Key | Meaning |
|---|---|
| `id` | The quest's id, typed in commands |
| `name`, `description` | `@key` or plain words; without them the id |
| `rarity` | Which bucket it is drawn from |
| `rule` | `required` and `order`, overriding the group's |
| `tasks` | A list; each needs `id`, `type` and `amount`, and may carry `description` and `feedback` |
| `rewards` | [Action lines](rewards.md) |
| `category`, `requires`, `cooldown` | [Catalogue](catalogues.md) quests only |
| `feedback` | This quest's own [feedback](feedback.md) |

## Amounts

A number, a span such as `"10-25"`, or a block with `least` and `most`. A span is drawn once when the quest is handed
out and stored with it, so a reload never changes the number under a player halfway through.

## Options

Every other key of a task is an option its [type](task-types.md) understands. Three shapes, three meanings:

| Written | Means |
|---|---|
| `block = STONE` | One block |
| `block = [STONE, GOLD_ORE]` or `block { any-of = [...] }` | A filter: any of them counts, and the quest is one count |
| `block { one-of = [STONE, GOLD_ORE] }` | A draw: one is chosen per player when the quest is handed out and stored with it |

Values are upper-cased, except `placeholder`, `permission` and `value`, which are read as written. `reverse = true`
takes a count back when an event undoes what the task counted, such as a block put back after it was broken.

## Names

A quest or task without a name or description is called by its id, or by the sentence its type carries under
`task.<type>` in `messages/`. Block, item and mob names are translated through the `vanilla` table of `messages/`; a
name the table lacks is spelled out from the server's own, so `DEEPSLATE_DIAMOND_ORE` reads "Deepslate Diamond Ore".

Related: [Task types](task-types.md), [Group files](../config/group-files.md)
