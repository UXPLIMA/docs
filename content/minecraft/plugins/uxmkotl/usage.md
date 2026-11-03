---
title: "Guide"
order: 10
description: "How uxmKotl works, how to set it up and what each file holds."
---

uxmKotl runs King of the Ladder: players climb to a plate, whoever stands on it is king, and the others knock them
off with a stick. The time each player holds the crown, their longest reign, their claims and their kills are kept
and ranked on boards.

## Install

Put `uxmKotl-<version>.jar` in `plugins/` and start the server. The first start writes `config.conf`,
`commands.conf`, `theme.conf`, the menus and the message files into `plugins/uxmKotl/`. Arenas are kept one file
each under `arenas/`. `/uxmkotl reload` reads the files again.

## Making an arena

1. `/uxmkotl create <arena>` writes a new arena, empty and switched off.
2. Set its pieces with `/uxmkotl set <arena> <piece> <value>`, or in the editor with `/uxmkotl edit <arena>`.
   `here` as the value takes the block you stand on.
3. `/uxmkotl enable <arena>` switches it on. `/uxmkotl show <arena>` prints every piece.

| Piece | Meaning |
|---|---|
| `corner1`, `corner2` | Two opposite corners of the arena's box, in any order. Both corners belong to it. |
| `plate` | The block a player stands on to be king. It must be inside the box. |
| `spawn` | Where a joining player is put. |
| `exit` | Where a leaving player is put. |
| `world` | The world the box, the plate and the spawn are in. |
| `name` | The name players read. |
| `join-on-enter` | Whether walking into the box joins a player. `/kotl join` works either way. |
| `clear-inventory`, `clear-effects`, `mode` | What happens on entry: the inventory and effects are cleared, and the game mode is set (adventure out of the box). |

## How a round works

- A player who joins gets the stick in `stick.slot`, or the first free slot.
- Stepping on the plate of an empty throne makes the player king at once. A sitting king is protected for
  `crown.claim-cooldown-ms`; until then another player on the plate cannot take the crown, and with
  `plate.repel.enabled` they are pushed back. After it, the crown changes hands and the old king's reign is added
  to their record.
- The king cannot claim the crown again. Leaving the arena ends the king's reign.
- A claim and a kill pay `rewards.claim` and `rewards.kill`, which pay nothing out of the box.

## Configuration

| Key | Default | Meaning |
|---|---|---|
| `crown.claim-cooldown-ms` | `1500` | How long a new king is protected. |
| `crown.ring` | on, gold | The particle ring around the king's head: `colour`, `radius`, `height`, `points`, `size`, `period-ms`. |
| `boundary.outline` | on | The particle outline an operator sees while marking a box. |
| `plate.repel` | off | Pushes back a player whose claim was refused: `strength`, `upward`. |
| `stick.material`, `slot`, `knockback-level` | stick, 0, 2 | The weapon. Its name and lore are in `messages/`. |
| `rewards.claim`, `rewards.kill` | nothing | `money` lines such as `"vault 250"` and console `commands` with `%player%`. |
| `leaderboard` | all four periods | `size`, `refresh-seconds`, `periods`, `time-zone`, `week-starts`, `keep-windows`. |
| `storage` | SQLite | Where the statistics are kept. |

## Commands

| Command | Permission | What it does |
|---|---|---|
| `/uxmkotl` |  | Administer King of the Ladder. |
| `/uxmkotl create <arena> [world]` | `uxmkotl.admin.create` | Write a new arena, empty and switched off. |
| `/uxmkotl set <arena> <piece> <value>` | `uxmkotl.admin.edit` | Set one piece of one arena. |
| `/uxmkotl show <arena>` | `uxmkotl.admin.list` | Print every piece of one arena, one per line, set or not. |
| `/uxmkotl edit <arena>` | `uxmkotl.admin.edit` | Open the editor for an arena. Players only. |
| `/uxmkotl enable <arena>` | `uxmkotl.admin.enable` | Switch an arena on. |
| `/uxmkotl disable <arena>` | `uxmkotl.admin.disable` | Switch an arena off. |
| `/uxmkotl delete <arena>` | `uxmkotl.admin.delete` | Delete an arena. |
| `/uxmkotl reset <player>` | `uxmkotl.admin.reset` | Take a player off the record and every board. |
| `/uxmkotl list` | `uxmkotl.admin.list` | Every arena, as a menu for a player and as lines for the console. |
| `/uxmkotl tp <arena>` | `uxmkotl.admin.tp` | Stand at the spawn of an arena. Players only. |
| `/uxmkotl reload` | `uxmkotl.admin.reload` | Read the configuration and the content again. |
| `/uxmkotl version` | `uxmkotl.admin.version` | Read the version this server runs. |
| `/uxmkotl doctor` | `uxmkotl.admin.doctor` | Say how this plugin is, in one line per thing that can be wrong. |
| `/kotl` | `uxmkotl.play` | Play King of the Ladder. |
| `/kotl join <arena>` |  | Enter an arena. Players only. |
| `/kotl leave` |  | Leave the arena you are in. Players only. |
| `/kotl stats` |  | Your own record. |
| `/kotl top [board] [period]` |  | The best players. |
| `/kotl lang` | `uxmkotl.language` | Say what language this player reads, and what else this server has. Players only. |
| `/kotl lang set <language>` | `uxmkotl.language` | Read this plugin in one language, whatever the client is set to. Players only. |
| `/kotl lang reset` | `uxmkotl.language` | Follow the client again, as a player who never chose anything does. Players only. |
| `/kotl lang set-player <player> <language>` | `uxmkotl.admin.language` | Set the language another player reads. |
| `/kotl lang reset-player <player>` | `uxmkotl.admin.language` | Clear the language another player chose. |

The `/kotl` subcommands need `uxmkotl.play`. All nodes and their defaults are in [reference.md](reference.md).

## Placeholders

| Placeholder | Value |
|---|---|
| `%uxmkotl_king_<arena>%` | The king of that arena, or the word for nobody. |
| `%uxmkotl_reign_<arena>%` | How long that king has held it, or empty. |
| `%uxmkotl_arena%` | The arena the player is in, or empty. |
| `%uxmkotl_reign%` | The player's own running reign, or empty. |
| `%uxmkotl_held%` | Their total time held. |
| `%uxmkotl_longest%` | Their longest single reign. |
| `%uxmkotl_claims%` | How many crowns they have taken. |
| `%uxmkotl_kills%` | How many players they have knocked off. |
| `%uxmkotl_top_<board>_<period>_<n>_name\|value\|head%` | Place `n` of a board. |
| `%uxmkotl_rank_<board>_<period>%` | The reader's own place on it. |
| `%uxmkotl_value_<board>_<period>%` | The reader's own value on it. |
| `%uxmkotl_resets_<period>%` | The time until the period starts again. |
| `%uxmkotl_head%` | What a `<head:...>` tag draws the reader from. |

The boards are `held`, `longest`, `claims` and `kills`; the periods `alltime`, `monthly`, `weekly` and `daily`.

## Examples

**Pay the king.** Set `rewards.claim.money = ["vault 50"]`.

**A walk-in arena.** `/uxmkotl set hill join-on-enter true`: players join by walking into the box.

**Let the lobby PvP plugin leave the arena alone.** In uxmHubPvP, set `arena.hands-off-when = ["%uxmkotl_arena%"]`.
