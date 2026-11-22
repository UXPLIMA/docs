---
title: Combat
order: 1316
description: A combat tag that stops players from teleporting, flying or hiding in a safe land mid-fight.
icon: swords
---

A player who hits another player, or is hit by one, is tagged for a short while. Until the tag runs out they
cannot teleport away, use the commands you list, fly, or walk into a land where PvP is off. Each new hit
restarts the time, and `/claim combat` shows how long is left.

Module `combat` · disabled by default · `modules/combat/config.yml`

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/claim combat` | Shows whether you are tagged and for how many seconds. | none |
| `/uxm combat untag <player>` | Ends an online player's tag at once. | `uxmclaims.command.combat.admin` |

The local names of these commands are in `modules/combat/lang/<language>/commands.yml`.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.bypass.combat` | nobody | Never tagged, and no combat rule applies. Not even operators have it unless you give it. |
| `uxmclaims.command.combat.admin` | op | `/uxm combat untag`. |

## Settings

| Key | Default | What it does |
|---|---|---|
| `tag.time` | `15s` | How long a tag lasts after the last hit (`30s`, `1m`). |
| `tag.attacker` | `true` | Tag the player who hits. |
| `tag.victim` | `true` | Tag the player who is hit. |
| `deny-teleport` | `true` | Block teleports by commands and plugins (warps, `/claim spawn`, `/home`, `/tpa`) while tagged. |
| `deny-ender-pearl` | `false` | Also block ender pearls and chorus fruit while tagged. |
| `deny-flight` | `true` | Stop tagged players from flying. A tag also takes a flying player out of the air. Creative and spectator players are left alone. |
| `deny-entering-pvp-off-lands` | `true` | Stop tagged players from walking into a land where PvP is off, so a fight cannot be escaped into a safe land. |
| `blocked-commands` | `spawn`, `home`, `homes`, `tpa`, `tpahere`, `tpaccept`, `back`, `warp`, `rtp`, `claim spawn`, `claim warp`, `claim unstuck`, `town spawn`, `nation spawn` | Commands a tagged player cannot use. An entry blocks the command and its subcommands: `claim spawn` blocks `/claim spawn`, `spawn` blocks `/spawn` and `/essentials:spawn`. |
| `kill-on-quit` | `false` | Kill a player who leaves the server while tagged. Always off on Folia. |
| `action-bar` | `true` | Show the seconds left in the action bar while tagged. |

The texts, including the action bar line, are in `modules/combat/lang/<language>/messages.yml`; `<seconds>` is
the time left.

## Notes

- A hit by a projectile tags the player who shot it. Hits on yourself do not count.
- "PvP off" means the land's `lands:pvp` flag is not set to allow; an unset flag counts as off. During an active
  [War](war.md), a land that is a battlefield for the player counts as PvP on, so attackers can follow defenders
  in. World PvP rules are not consulted.
- A tagged player already inside a PvP-off land may move around in it and leave it.
- `blocked-commands` matches the words as typed. A local command name or an alias from the
  [Aliases](aliases.md) module is a different word: list it too if players should not use it in combat.
- Tags live in memory. Death, quitting and a restart end them.
- Other modules ask this one before their own teleports: [Warps](warps.md), `/claim spawn` and
  [Random Teleport](rtp.md) refuse a tagged player.

Related: [Newbie Protection](newbie-protection.md), [War](war.md), [Lands](lands.md)
