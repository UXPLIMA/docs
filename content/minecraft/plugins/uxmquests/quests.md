---
title: "Quests and groups"
order: 40
description: "A group file under `groups/` is one set of quests."
---

A group file under `groups/` is one set of quests. The file name is the id of the group, so `weekly.conf` is a
group named `weekly`. Copy `daily.conf` to start. Both shapes below use the same file format and the same
window.

Contents: [A group](#a-group) · [Resets](#resets) · [Catalogue](#catalogue) · [Quests and tasks](#quests-and-tasks) ·
[Rewards](#rewards) · [Milestones](#milestones) · [Shared boards](#shared-boards) · [Feedback](#feedback)

## A group

```hocon
group {
  name        = "@group.daily.name"   # a leading @ names a key of messages/
  description = "@group.daily.description"
  mode        = cycle                 # cycle or catalogue
  count       = 4                     # quests on a board
  claim       = automatic             # automatic or manual
  sharing     = solo                  # solo or contribution
  rerolls     = 1                     # quests a player may swap per board
  rule        { required = 2, order = any }
  reset       { type = wall-clock, at = "04:00", zone = "Europe/Istanbul" }
}
rarities = [
  { name = common, weight = 10 }
  { name = rare,   weight = 2, least = 1, most = 2 }
]
quests = [ ... ]
```

| Key | Meaning |
|---|---|
| `mode = cycle` | A board of `count` quests is drawn for each player and thrown away on every reset. |
| `mode = catalogue` | Every quest is on offer and the player starts the ones they want. |
| `claim = automatic` | A finished quest pays at once. |
| `claim = manual` | A finished quest waits until the player runs `/quests claim` or clicks it. |
| `rule.required` | How many of a quest's tasks finish it. Left out, all of them. |
| `rule.order` | `any`, or `in-order` for a quest whose second task waits for the first. |
| `rarities` | The buckets a quest is drawn from. `weight` is how often a bucket is picked, `least` how many of it every board is guaranteed, `most` a ceiling. |

A quest writes its own `rule` to override the group's. The quests are a list, never a block, because a
block loses the order and a sequential quest runs in it.

## Resets

| `reset.type` | A new board is drawn |
|---|---|
| `wall-clock` | At `at`, every day, in `zone`. Everybody at once. |
| `rolling` | `every` (default `24h`) after the player's own board was handed out. |
| `cron` | By the five-field expression in `cron`, in `zone`. |
| `never` | Never. What a catalogue writes. |

The timezone is always read from the file, never from the machine, so moving the server to another host
does not move the reset.

## Catalogue

`mode = catalogue` with `reset { type = never }`. A catalogue quest is not drawn and does not reset. Three
keys belong to catalogue quests:

```hocon
{
  id       = build-a-shelter
  category = building          # what the window sorts it under
  requires = [ gather-wood ]   # not offered until these are finished
  cooldown = 12h               # how long after payment it may be taken again
}
```

Players start one with `/quests start <group> <quest>` or from the window.

## Quests and tasks

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

A task needs `id`, `type` and `amount`. Every other key is an option the type understands, see
[objectives](objectives.md). A task may also carry `description` and `feedback`.

An `amount` is a number, a span such as `"10-25"`, or a block with `least` and `most`. A span is drawn once
when the quest is assigned and stored with the assignment, so a reload never changes the number under a
player who is halfway through.

An option is written in three shapes with three meanings:

| Written | Means |
|---|---|
| `block = STONE` | One block. |
| `block = [STONE, GOLD_ORE]` or `block { any-of = [...] }` | A filter: any of them counts, and the quest is one count. |
| `block { one-of = [STONE, GOLD_ORE] }` | A draw: one is chosen per player when the quest is assigned and stored with it. |

Values are upper-cased, except the `placeholder`, `permission` and `value` options, which are read as
written.

A quest or task that writes no name or description is called by its id, or by the sentence its type carries
under `task.<type>` in `messages/`. Block, item and mob names are translated through the `vanilla` table of
`messages/`; a name it lacks is spelled out from the server's own, so `DEEPSLATE_DIAMOND_ORE` reads as
"Deepslate Diamond Ore".

## Rewards

A reward is a list of action lines, the same grammar every UXPLIMA plugin reads. A single line without a list
is read as one line.

- Verbs include `[message]`, `[broadcast]`, `[actionbar]`, `[title]`, `[subtitle]`, `[bossbar]`, `[particle]`,
  `[sound]`, `[effect]`, `[console]` and `[player]`, and `[take-money]` and `[give-money]` through the
  server's economy. Any of them may carry `{delay=, chance=, repeat=, every=, if=}` between the verb and the
  payload.
- `{player}`, `<player>`, `%player%` and `%player_name%` are the player's name, filled in before the line is
  read. A name an account could not have is never filled in: that line is skipped and the console says so.
- `{50-200}` is a span drawn once per payment, so the number shown is the number paid.
- `@some.key` names a line of `messages/`, read in the paid player's language.

A group file may also hold a top-level `rewards` block with a `set` list, paid once when every quest on a
board is finished, and a `milestones` list.

## Milestones

A milestone pays when a total of finished quests is reached. A group file holds the milestones of its own
group, and `rewards.milestones` in `config.conf` counts a player's whole record:

```hocon
rewards {
  milestones = [
    { at = 100, rewards = [ "[message] One hundred quests." ] }
    { at = 500, rewards = [ "[console] lp user {player} parent add veteran" ] }
  ]
}
```

A group file writes the same `milestones` list inside its own top-level `rewards` block, beside `set`.

## Shared boards

`sharing = contribution` makes the group's board one row the whole server works on. Each player's work is
counted separately and the reward follows the count, so standing next to a friend pays nothing. A fresh board
clears the record of who did what.

## Feedback

Four moments tell a player what happened: `started`, `progressed`, `completed` and `paid`. Each is a list of
action lines with no ceiling:

```hocon
feedback {
  completed = [
    "[subtitle] @feedback.completed.title | @feedback.completed.subtitle 0.5 2.5 0.5"
    "[toast] @feedback.completed.toast"
    "[sound] block.note_block.pling 0.6 1.5"
  ]
}
```

Two verbs are this plugin's own: `[toast]` (the advancement popup; icon and frame are `feedback.toast-icon`
and `feedback.toast-frame`) and `[questbar] <seconds> [colour] [style] | <text>` (a boss bar filled to how far
the quest has got). `<quest>`, `<group>`, `<done>`, `<needed>` and `<percent>` are filled on every line.

The same `feedback { }` block may sit in `config.conf`, in a group, in a quest and in a task. The first level
that answers for a moment wins, and a list that is present stops the fallback even when it is empty, which is
how one noisy task is silenced:

```hocon
tasks = [ { id = dig, type = block-break, amount = 500, feedback { progressed = [] } } ]
```

A moment written as the older block of channel switches is still read, as the list it describes.
