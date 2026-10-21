---
title: Commands
order: 20
description: The admin command and its subcommands, with tab completion and the one-way convert.
icon: terminal
---

uxmSpawners has one command, and everything under it is for staff. Players never type anything:
they click spawners.

`/uxmspawners`, aliases `/spawners`, `/uxmspawner`, `/usp`. Every subcommand requires
`uxmspawners.admin`.

| Command | What it does |
|---|---|
| `/uxmspawners give <player> <type> [amount]` | Gives spawner items. `type` is a key from the `spawners` section or an entity name |
| `/uxmspawners giveboost <player> <boost> [amount]` | Gives a boost item defined in `modules/boosts.yml` |
| `/uxmspawners givepickaxe <player> [amount]` | Gives the spawner pickaxe from `modules/other.yml → break-management` |
| `/uxmspawners reload` | Reloads config, modules, languages and loot tables |
| `/uxmspawners convert` | Converts existing WildStacker spawners placed via SuperiorSkyblock |

Tab completion is live: the second argument suggests online players, and the third suggests the
spawner types you have configured plus every entity name, or the boost ids you have defined.

## Giving spawners

```
/uxmspawners give Notch zombie 5
/uxmspawners give Notch diamond 1
```

The type is looked up first against the keys in the `spawners` section of `config.yml`, then
against entity names. Anything that fits goes into the player's inventory; anything that does not
fit is dropped at their feet and both of you are told how much.

<Callout type="warning" title="convert is a one-way operation">

`/uxmspawners convert` rewrites spawner data in place. Take a backup of the world and the plugin
folder before you run it, and run it once, not once per restart.

</Callout>
