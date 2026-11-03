---
title: Setting up the fight
order: 204
description: The settings a lobby changes first, and a test with two players.
icon: flag
---

uxmHubPvP works as shipped. These are the settings a lobby usually changes, in the order most servers reach them.
Every one is in `config.conf` or `kit.conf`, and `/uxmhubpvp reload` applies it.

## 1. Give the toggle item its slot

```hocon
toggle-item {
  slot = 4
  in-pvp { slot = 8 }
}
```

`slot` is where the sword goes on join; `in-pvp.slot` is where the red dye that turns PvP off sits on top of the kit.
Pick slots no other plugin of the lobby uses. To hand the item out yourself, set `give-on-join = false`; to have no
item at all and only `/pvp`, set `enabled = false`. See [Toggle item](../fighting/overview.md#the-toggle-item).

## 2. Say where a dead fighter wakes up

```hocon
respawn {
  instant = true
  spawn { world = "world", x = 0.5, y = 64.0, z = 0.5, yaw = 0.0, pitch = 0.0 }
}
```

With `world` blank, as shipped, the server or your lobby plugin decides. Write the lobby spawn here if nothing else
does. See [Deaths and kills](../fighting/deaths-and-kills.md#respawning).

## 3. Shape the kit

`kit.conf` holds iron armour, a diamond sword with Sharpness I, a shield, five golden apples and sixteen steaks.
Delete a piece to leave it out, or change a material. See [kit.conf](../config/kit-conf.md).

## 4. Decide what a kill pays

```hocon
rewards {
  money = ["exp 10", "vault 25"]
}
```

See [Rewards](../fighting/rewards.md).

## 5. Open the commands a fighter may type

A tagged fighter can type only `pvp`, `msg`, `r`, `reply` and `helpop`. Add your own, such as the alias `fight` of
`/pvp` or a party chat command:

```hocon
combat-tag {
  allowed-commands = ["pvp", "fight", "msg", "r", "reply", "helpop", "party"]
}
```

See [Combat tag](../fighting/combat-tag.md).

## 6. Step aside for other games

If the lobby runs uxmKotl or another game where players hit each other without turning PvP on, name the game's
placeholder so this plugin leaves those hits alone:

```hocon
arena {
  hands-off-when = ["%uxmkotl_arena%"]
}
```

This needs PlaceholderAPI. See [Other games](../integrations/other-games.md).

## 7. Test it

1. With two players, click the sword with both and stand still for three seconds. Each gets the kit and glows.
2. Hit each other: both read *You are in a fight for 10 seconds*, and `/spawn` is refused.
3. Kill one: the killer reads the kill line and earns the reward; the other respawns with their own items and PvP off.
4. `/pvp stats` shows the kill and the death. `/pvp top` shows the board after 15 seconds.

A player who is not fighting cannot hit a fighter, and the other way round; the attacker reads why.

Related: [config.conf](../config/config-conf.md), [kit.conf](../config/kit-conf.md), [Concepts](concepts.md)
