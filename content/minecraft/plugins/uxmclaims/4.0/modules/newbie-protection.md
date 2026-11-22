---
title: Newbie Protection
order: 1317
description: A PvP shield for first-time players, and a short shield for everyone after a respawn.
icon: shield-plus
---

Players who join the server for the first time cannot be hurt by other players for a while, two hours of play by
default. They can end the shield early with `/claim protection off`, and attacking another player ends it too.
After any respawn, every player also gets a few seconds in which other players cannot hurt them.

Module `newbie-protection` · disabled by default · `modules/newbie-protection/config.yml`

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/claim protection` | Shows how long your protection lasts. | none |
| `/claim protection off` | Ends your protection now, when `allow-self-disable` is on. | none |
| `/uxm protection give <player> [duration]` | Gives an online player protection, for `pvp-protection.duration` unless you name a duration such as `30m` or `2h`. | `uxmclaims.command.newbie-protection.admin` |
| `/uxm protection remove <player>` | Ends an online player's protection. | `uxmclaims.command.newbie-protection.admin` |

The local names of these commands are in `modules/newbie-protection/lang/<language>/commands.yml`.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.newbie-protection.admin` | op | `/uxm protection give` and `remove`. |

## Settings

| Key | Default | What it does |
|---|---|---|
| `pvp-protection.duration` | `2h` | How long a new player is protected (`30m`, `2h`); `0s` turns new-player protection off. |
| `pvp-protection.count` | `playtime` | How the time runs down. `playtime`: only while the player is online. `realtime`: also while they are offline. |
| `pvp-protection.allow-self-disable` | `true` | Whether players may end their own protection early with `/claim protection off`. |
| `pvp-protection.on-attack` | `end` | What happens when a protected player hits another player. `end`: their protection ends and the hit goes through. `deny`: the hit is blocked, so protected players cannot attack. |
| `respawn-protection` | `10s` | After a respawn, other players cannot hurt the player for this long; `0s` turns it off. |

The texts are in `modules/newbie-protection/lang/<language>/messages.yml`; `<time>` is the protection left.

## Notes

- Only a player's very first join on this server starts the protection. Turning the module on later does not
  protect players who have played before; use `/uxm protection give` for them.
- The time left is saved when the player quits and every minute while they play, and is shown again when they
  come back.
- With `on-attack: end`, an arrow or other projectile that hits a player ends the shooter's protection too.
- Protection works through the claim protection check for PvP, so it shields a player in the wilderness and in
  lands alike. Respawn protection counts as protection for that check.
- Admin give and remove work on online players only. In network mode, a give or remove on another server reaches
  a player who is online here only at their next join.
- There is no first-claim hint and no message when respawn protection starts or ends yet.

Related: [Combat](combat.md), [War](war.md), [Lands](lands.md)
