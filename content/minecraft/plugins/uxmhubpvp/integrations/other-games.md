---
title: Other games
order: 702
description: Leaving the hits of another plugin's game to that game, with hands-off-when.
icon: puzzle
---

`arena.only-fighters-may-fight` refuses a hit unless both players turned PvP on. That is right in a lobby and wrong
inside another plugin's game: two players in a round of uxmKotl never turned PvP on, and they are not meant to,
because the round is the agreement. Without a guard their knockback sticks hit nothing, and no line says why.

```hocon
arena {
  hands-off-when = ["%uxmkotl_arena%"]
}
```

Each entry is a placeholder that names the game a player is in. For a hit, it is filled for the attacker and for the
victim. When both answer, and answer the same thing, the hit is that game's: uxmHubPvP neither refuses it nor tags
anybody, and the world and the other plugin decide.

| Attacker answers | Victim answers | The hit |
|---|---|---|
| `hill` | `hill` | Left to the game |
| `hill` | `tower` | Judged by uxmHubPvP: two different games |
| `hill` | nothing | Judged by uxmHubPvP: a lobby player punching someone in a round is still refused |
| nothing | nothing | Judged by uxmHubPvP |

An answer of nothing, `0`, `-`, `false` or `no` means "in no game". A placeholder no plugin on the server owns comes
back as written and also means no game, so a guard for a game that is not installed changes nothing. The first entry
that matches decides; the rest are not read.

The guards need PlaceholderAPI. Without it no guard ever matches, and every hit is judged by the fighter rule.

## Examples

| Game | Guard |
|---|---|
| uxmKotl | `"%uxmkotl_arena%"` |
| A game whose placeholder names the arena a player is in | `"%game_arena%"` |
| A plugin that answers `true` or `false` | Not enough on its own: both players answer `true` in two different rounds. Use a placeholder that names the round |

Related: [config.conf](../config/config-conf.md#arena), [Turning PvP on and off](../fighting/overview.md#who-may-hit-whom)
