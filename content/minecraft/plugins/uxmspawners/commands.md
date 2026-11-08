---
title: Commands
order: 120
description: The admin command, what each subcommand does, and the one-way WildStacker convert.
icon: terminal
---

uxmSpawners has one command, and all of it is for staff. Players never type anything: they click
spawners.

`/uxmspawners`, or `/spawners`, `/uxmspawner`, `/usp`. Each subcommand has a permission of its own,
and `uxmspawners.admin` holds them all. See [Permissions](permissions.md#staff).

| Command | What it does |
|---|---|
| `/usp` or `/usp help [page]` | Lists the subcommands you may use, six to a page |
| `/usp give <player> <spawner> [amount]` | Gives spawner items |
| `/usp giveboost <player> <boost>` | Gives one boost item from `boosts.yml` |
| `/usp givepickaxe <player> [uses]` | Gives the spawner pickaxe, with 10 uses unless you say otherwise |
| `/usp reload` | Reads every file again |
| `/usp debug` | Shows why the spawner you look at does what it does |
| `/usp bypass` | Turns bypass mode on or off: you can use every spawner as its owner |
| `/usp convert` | Turns WildStacker spawners on SuperiorSkyblock2 islands into uxmSpawners spawners |

A player without any of these permissions gets no answer at all, not even a "no permission"
message. The target player has to be online; what does not fit in their inventory drops at their
feet.

## give

```
/usp give Notch zombie 5
/usp give Notch diamond
```

`spawner` is looked up first among the files in `spawners/`, without regard to case, then among mob
names. A mob without a file of its own gets a spawner built from `default.yml`. Only mobs can be
given: a name like `ARROW` or `PLAYER` is refused. Tab completion offers your files and every mob.
`amount` is capped at what one inventory holds.

## giveboost and givepickaxe

`giveboost` gives one boost per use; there is no amount. `givepickaxe` gives one pickaxe with `uses`
uses, or 10. See [Boosts](features/boosts.md) and [Breaking Rules](features/breaking.md#the-spawner-pickaxe).

## reload

`reload` applies changes to every file: spawner types, prices and the price plugin, boosts, loot
tables, holograms, the language and the menus. Spawners already placed keep their owner, level and
storage. If a file has an error, the reload is refused, the console says what is wrong, and the
previous settings stay in use.

A restart is needed for the database settings (except `flush-interval-seconds`), the license key,
and plugins you install or remove: a claim plugin, WildLoaders or PacketEvents.

Spawner items already handed out keep the name and lore they were made with; only new items show a
changed name.

## debug

Look at a spawner within 6 blocks and run `/usp debug`. The answer covers its owner and friends, its
type and level, what it produces and when it last did, the nearest player and chunk loader, its
storage and limits, the sell price source and your multiplier, its boosts, the claim plugin and its
hologram. It is the first thing to run when a spawner does not do what you expect.

## bypass

Bypass mode lets you open and manage every spawner as if it were yours: its menus, storage, friends,
levels and the stop button. It does not let you break other players' spawners. It ends when you run
the command again or lose `uxmspawners.bypass`. Staff who right-click someone else's spawner are
reminded that the mode exists.

## convert

`convert` needs WildStacker and SuperiorSkyblock2. Every WildStacker spawner inside the protected
range of an island becomes a uxmSpawners spawner where it stands: the same mob, owned by the island's
owner, at a level equal to its stack size, with Auto Kill on and both storages off. Spawners outside
an island, or outside its range, are skipped, and the console lists each one with the reason.

<Callout type="warning" title="convert cannot be undone">

Back up the worlds and the plugin folder first. A run stopped half way through loses nothing:
running it again converts the rest.

</Callout>

Related: [Permissions](permissions.md), [Boosts](features/boosts.md), [Breaking Rules](features/breaking.md)
