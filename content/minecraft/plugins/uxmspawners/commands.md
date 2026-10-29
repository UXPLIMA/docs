---
title: Commands
order: 20
description: The admin command, what each subcommand gives, and the one-way WildStacker convert.
icon: terminal
---

uxmSpawners has one command, and all of it is for staff. Players never type anything: they click
spawners.

`/uxmspawners`, or `/spawners`, `/uxmspawner`, `/usp`. Every subcommand needs `uxmspawners.admin`.

| Command | What it does |
|---|---|
| `/uxmspawners give <player> <type> [amount]` | Gives spawner items |
| `/uxmspawners giveboost <player> <boost>` | Gives one boost item from `modules/boosts.yml` |
| `/uxmspawners givepickaxe <player> <uses>` | Gives the spawner pickaxe from `modules/other.yml`, with that many uses |
| `/uxmspawners reload` | Reads the config, the modules, the language file and the loot tables again |
| `/uxmspawners convert` | Turns WildStacker spawners on SuperiorSkyblock2 islands into uxmSpawners spawners |

The player has to be online. What does not fit in their inventory drops at their feet.

## give

```
/uxmspawners give Notch zombie 5
/uxmspawners give Notch diamond
```

`type` is looked up first among your spawner types in `config.yml`, written exactly as the key, then
among mob names. A mob without a type of its own gets a spawner built from `default`. Tab completion
offers your types and every mob.

## giveboost and givepickaxe

`giveboost` gives one boost per use; there is no amount. `givepickaxe` needs the number of uses:
each spawner broken with the pickaxe takes one, and it disappears at zero. A number that cannot be
read gives a pickaxe with one use.

## reload

`reload` applies changes to every file, including new spawner types, prices, the language and
holograms. Switching the price source back to `CUSTOM` is the exception: it needs a restart. Spawners already placed keep their owner, level and storage.

Spawner items already handed out keep the name and lore they were made with; only new items show a
changed name.

## convert

`convert` needs WildStacker and SuperiorSkyblock2. Every WildStacker spawner on an island is removed
and placed again as a uxmSpawners spawner of the same mob, owned by the island's owner, at a level
equal to its stack size.

<Callout type="warning" title="convert cannot be undone">

It breaks every WildStacker spawner and places a new block in its spot. Back up the worlds and the
plugin folder first, and run it once, with no players online.

</Callout>

The messages these commands send are in the language file; from the console they are short English
lines.

Related: [Permissions](permissions.md), [Boosts](features/boosts.md), [Breaking Rules](features/breaking.md)
