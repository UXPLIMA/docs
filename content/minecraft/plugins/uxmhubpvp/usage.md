---
title: "Guide"
order: 10
description: "How uxmHubPvP works, how to set it up and what each file holds."
---

uxmHubPvP lets players fight in the lobby when they choose to. A player turns PvP on with the sword in their hotbar
or `/pvp`; after a short countdown their own items are stored safely and they get the kit. Turning PvP off takes
longer, a hit marks both players for a while, and kills are counted on boards.

## Install

Put `uxmHubPvP-<version>.jar` in `plugins/` and start the server. The first start writes `config.conf`,
`commands.conf`, `theme.conf` and the message files into `plugins/uxmHubPvP/`. Players get the toggle item in
hotbar slot 2 when they join. `/uxmhubpvp reload` reads the files again.

## How a fight works

- **Turning on.** The countdown takes `warm-up.enter-ms`. Moving more than `warm-up.move-limit` blocks from where
  it started stops it. When it ends, the player's items are written to the database first, and only then is the
  kit handed out.
- **Fighting.** With `arena.only-fighters-may-fight` on, a hit counts only when both players have PvP on. Each hit
  marks both players for `combat-tag.duration-ms`; a new hit extends the mark. A marked player may type only the
  commands in `combat-tag.allowed-commands`, matched by their name without slash, namespace or arguments, so
  `/minecraft:tell` counts as `tell`.
- **Turning off.** The countdown takes `warm-up.leave-ms`, longer on purpose, and is cancelled by moving away.
  The player counts as fighting until it ends.
- **Leaving.** Turning off, dying, disconnecting and a server stop all give the stored items back. A row is
  dropped only after the items are back, so a crash leaves it behind and the next join hands the items over.
- **Kills.** A kill adds to the killer's kills and streak and pays `rewards.money`; the victim's deaths go up and
  their streak goes back to zero. The best streak is kept.

## Other plugins' games

`arena.hands-off-when` lists PlaceholderAPI texts that name the game a player is in, such as an arena id. When both
players' answers say something and say the same thing, the hit belongs to that game and this plugin does not judge
it. Empty answers, `0`, `-`, `false`, `no` and a placeholder no plugin fills mean "in no game".

## Configuration

| Key | Default | Meaning |
|---|---|---|
| `arena.only-fighters-may-fight` | `true` | Refuses hits unless both players have PvP on. |
| `arena.hands-off-when` | `[]` | Games this plugin leaves alone, see above. |
| `warm-up.enter-ms`, `leave-ms` | `3000`, `8000` | The countdowns in and out. |
| `warm-up.move-limit` | `1.5` | How far a player may move during a countdown. |
| `warm-up.show-action-bar` | `true` | Shows the countdown on the action bar. |
| `combat-tag.duration-ms` | `10000` | How long a hit marks both players. 0 turns the mark off. |
| `combat-tag.allowed-commands` | `pvp`, `msg`, `r`, `reply`, `helpop` | The only commands a marked player may run. |
| `toggle-item` | slot 2, iron sword | The hotbar item: `enabled`, `give-on-join`, `slot`, `item`, and `in-pvp` for how it looks during a fight. |
| `kit` in `kit.conf` | iron armour, diamond sword, shield | The kit: `weapon-slot`, each armour piece, `weapon`, `off-hand` and `extras` by inventory slot. It has its own file, which a start never merges into, and a kit an earlier release kept in `config.conf` moves there on the first start. |
| `respawn.instant` | `true` | Skips the death screen. |
| `respawn.spawn` | server's | Where a killed player wakes up. A blank world lets the server decide. |
| `visuals.glow` | `true` | Outlines fighting players. The colour comes from their scoreboard team, such as uxmGlow's. |
| `visuals.hit-particles` | `true` | Particles on a hit: `hit-particle` and `hit-particle-count`. |
| `rewards.money` | `["exp 10"]` | What a kill pays: `exp`, `levels`, `vault`, `vault-unlocked`, `playerpoints`, `ecobits:<currency>`, `treasury`. |
| `leaderboard` | all four periods | `size`, `refresh-seconds`, `periods`, `time-zone`, `week-starts`, `keep-windows`, as in the boards of the other UXM plugins. |
| `storage` | SQLite | Where kept inventories and statistics live. |

The `effects` lists at the end of `config.conf` say what entering, leaving, a kill, a death and the other moments do.

## Commands

| Command | Permission | What it does |
|---|---|---|
| `/uxmhubpvp` |  | Administer the plugin. |
| `/uxmhubpvp reset <player>` | `uxmhubpvp.admin.reset` | Take a player off the record and every board. |
| `/uxmhubpvp reload` | `uxmhubpvp.admin.reload` | Read the configuration and the content again. |
| `/uxmhubpvp version` | `uxmhubpvp.admin.version` | Read the version this server runs. |
| `/uxmhubpvp doctor` | `uxmhubpvp.admin.doctor` | Say how this plugin is, in one line per thing that can be wrong. |
| `/pvp` | `uxmhubpvp.use` | Fight in the lobby. |
| `/pvp stats [player]` | `uxmhubpvp.stats` | Show a record. |
| `/pvp top [board] [period]` | `uxmhubpvp.top` | Show the leaderboard. |
| `/pvp lang` | `uxmhubpvp.language` | Say what language this player reads, and what else this server has. Players only. |
| `/pvp lang set <language>` | `uxmhubpvp.language` | Read this plugin in one language, whatever the client is set to. Players only. |
| `/pvp lang reset` | `uxmhubpvp.language` | Follow the client again, as a player who never chose anything does. Players only. |
| `/pvp lang set-player <player> <language>` | `uxmhubpvp.admin.language` | Set the language another player reads. |
| `/pvp lang reset-player <player>` | `uxmhubpvp.admin.language` | Clear the language another player chose. |

All nodes and their defaults are in [reference.md](reference.md).

## Placeholders

| Placeholder | Value |
|---|---|
| `%uxmhubpvp_in_pvp%` | `true` or `false`. |
| `%uxmhubpvp_tagged%` | `true` or `false`. |
| `%uxmhubpvp_kills%` | The kill count. |
| `%uxmhubpvp_deaths%` | The death count. |
| `%uxmhubpvp_ratio%` | Kills over deaths, to two decimals. |
| `%uxmhubpvp_streak%` | The run of kills since the last death. |
| `%uxmhubpvp_best_streak%` | The longest run ever. |
| `%uxmhubpvp_top_<board>_<period>_<n>_name\|value\|head%` | Place `n` of a board. |
| `%uxmhubpvp_rank_<board>_<period>%` | The reader's own place on it. |
| `%uxmhubpvp_value_<board>_<period>%` | The reader's own value on it. |
| `%uxmhubpvp_resets_<period>%` | The time until the period starts again. |
| `%uxmhubpvp_head%` | What a `<head:...>` tag draws the reader from. |

The boards are `kills` and `streak`; the periods `alltime`, `monthly`, `weekly` and `daily`.

## Examples

**Pay real money per kill.** Set `rewards.money = ["vault 25"]`.

**Leave a minigame alone.** If a game plugin fills `%game_arena%` with the arena a player is in, set
`arena.hands-off-when = ["%game_arena%"]`.

**No escaping a fight.** Raise `warm-up.leave-ms` to `15000` and keep `combat-tag.allowed-commands` short.
