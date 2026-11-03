---
title: "Reference"
order: 60
description: "Every command, permission, placeholder and file of uxmCrates."
---

Command names below are the defaults; `commands.conf` renames, aliases and disables them. The player command
`/crates` has the aliases `/crate` and `/cr`. The operator command `/uxmcrates` has the aliases `/cratesadmin`
and `/uxmcr`.

## Player commands

A player command carries no permission node. It refuses with a message that says why, so a player who cannot do
something is told. Which crates a player may open is decided by each crate's `permission`, key and requirements.

| Command | What it does |
|---|---|
| `/crates` | Opens the crate list. |
| `/crates open <crate>` | Opens a crate by name, with nothing placed in the world. |
| `/crates preview <crate>` | Shows every reward of a crate with the player's real chance. Costs nothing. |
| `/crates keys` | Shows the keys the player holds and the crates each opens. |
| `/crates claim` | Opens the rewards that were decided and not yet delivered. |
| `/crates send <player> <key> <amount>` | Sends keys of a key file with `tradeable = true` to another player. |
| `/crates massopen <crate>` | Opens every key at once, up to `opening.bulk-cap`, with one summary screen. |
| `/crates cost <crate>` | Shows what a crate costs, line by line. |
| `/crates milestones [crate]` | Shows milestone progress. With no crate it shows the ladder that counts every crate. |

## Operator commands

| Command | Permission | What it does |
|---|---|---|
| `/uxmcrates` | `uxmcrates.admin.version` | Prints the usage. |
| `/uxmcrates key` | `uxmcrates.admin.key` | Prints the usage of the key branch. |
| `/uxmcrates key give <player> <key> <amount>` | `uxmcrates.admin.key.give` | Adds to a player's key balance. |
| `/uxmcrates key giveitem <player> <key> <amount>` | `uxmcrates.admin.key.giveitem` | Gives the physical key item. |
| `/uxmcrates key take <player> <key> <amount>` | `uxmcrates.admin.key.take` | Takes keys off a player. |
| `/uxmcrates key set <player> <key> <amount>` | `uxmcrates.admin.key.set` | Writes a player's balance. |
| `/uxmcrates key giveall <key> <amount>` | `uxmcrates.admin.key.giveall` | Gives every online player keys. |
| `/uxmcrates key giveoffline <player> <key> <amount>` | `uxmcrates.admin.key.giveoffline` | Gives keys to a player who is away. |
| `/uxmcrates key ledger <player>` | `uxmcrates.admin.key.ledger` | Reads a player's key movements, newest first. |
| `/uxmcrates place <crate>` | `uxmcrates.admin.place` | Puts a crate on the block being looked at. |
| `/uxmcrates tool <crate>` | `uxmcrates.admin.place` | Gives the link tool for a crate. |
| `/uxmcrates remove` | `uxmcrates.admin.remove` | Takes the crate off the block being looked at. |
| `/uxmcrates forceopen <player> <crate>` | `uxmcrates.admin.forceopen` | Opens a crate for an online player with no key and no wait. |
| `/uxmcrates envoy start\|stop <envoy>` | `uxmcrates.admin.envoy` | Starts or stops an envoy. |
| `/uxmcrates envoy list` | `uxmcrates.admin.envoy` | Lists envoys, whether they run and when the next is due. |
| `/uxmcrates reset cooldown <player> <crate>` | `uxmcrates.admin.reset.cooldown` | Clears a player's wait on a crate. |
| `/uxmcrates reset player-limit <player> <crate>` | `uxmcrates.admin.reset.player-limit` | Clears a player's win counts on a crate. |
| `/uxmcrates reset global-limit <crate>` | `uxmcrates.admin.reset.global-limit` | Clears the server's win counts on a crate. |
| `/uxmcrates reroll give <player> <amount>` | `uxmcrates.admin.reroll.give` | Grants rerolls. |
| `/uxmcrates log <field> <value>` | `uxmcrates.admin.log` | Reads the opening log by player, crate or reward. |
| `/uxmcrates editor` | `uxmcrates.admin.editor` | Opens the in-game editor. |
| `/uxmcrates migrate <plugin>` | `uxmcrates.admin.migrate` | Reads a crate set out of another plugin. |
| `/uxmcrates doctor` | `uxmcrates.admin.doctor` | Reports the health checks. |
| `/uxmcrates reload` | `uxmcrates.admin.reload` | Reads every file again. |
| `/uxmcrates version` | `uxmcrates.admin.version` | Prints the running version. |
| `/uxmcrates lang` | `uxmcrates.language` | Shows the language the player reads and the choices. |
| `/uxmcrates lang set <language>` | `uxmcrates.language` | Chooses a language. |
| `/uxmcrates lang reset` | `uxmcrates.language` | Follows the client's language again. |
| `/uxmcrates lang set-player <player> <language>` | `uxmcrates.admin.language` | Sets another player's language. |
| `/uxmcrates lang reset-player <player>` | `uxmcrates.admin.language` | Clears another player's choice. |

`/uxmcrates log` takes the field `player`, `crate` or `reward`. A player gets a paged window; the console gets
lines.

## Permissions

| Permission | Default |
|---|---|
| `uxmcrates.language` | everyone |
| `uxmcrates.broadcast.hide` | everyone: stops showing other players' wins. `broadcast.opt-out-permission` renames it. |
| `uxmcrates.cooldown.bypass` | op: opens a crate with no wait, timed or one-time |
| `uxmcrates.admin.*` | op: the family of operator permissions |
| `uxmcrates.admin.language` | op |
| `uxmcrates.admin.doctor`, `.version`, `.reload`, `.editor`, `.place`, `.remove`, `.forceopen`, `.log`, `.migrate`, `.envoy` | op |
| `uxmcrates.admin.key` and `.key.give`, `.key.giveall`, `.key.giveitem`, `.key.giveoffline`, `.key.set`, `.key.take`, `.key.ledger` | op |
| `uxmcrates.admin.reroll` and `.reroll.give` | op |
| `uxmcrates.admin.reset` and `.reset.cooldown`, `.reset.player-limit`, `.reset.global-limit` | op |

Three families are named by the operator and declared nowhere: the reroll group permissions in
`rerolls.groups` (the shipped examples are `uxmcrates.reroll.silver` and `uxmcrates.reroll.gold`), the luck
permissions in `luck.permission`, and a crate's or reward's own `permission`.

## Placeholders

PlaceholderAPI is required for these; the identifier is `uxmcrates`. A placeholder is asked per player and
answered from a copy held in memory, refreshed every `placeholders.refresh-seconds`.

| Placeholder | Answer |
|---|---|
| `%uxmcrates_keys_<crate>%` | The keys that crate takes, held by the player. |
| `%uxmcrates_keys_total%` | Every key the player holds. |
| `%uxmcrates_virtual_keys_<key>%` | One key's balance. |
| `%uxmcrates_opened_<crate>%` | How many times the player opened the crate. |
| `%uxmcrates_opened_total%` | How many times the player opened anything. |
| `%uxmcrates_cooldown_<crate>%` | The wait left, such as `6h 12m`. |
| `%uxmcrates_cooldown_seconds_<crate>%` | The same wait in whole seconds. |
| `%uxmcrates_pending%` | How many rewards are waiting. |
| `%uxmcrates_rerolls%` | How many rerolls the player was granted. |
| `%uxmcrates_milestone_<crate>%` | Openings left to that crate's next milestone. |
| `%uxmcrates_milestone%` | Openings left to the next milestone counted across every crate. |
| `%uxmcrates_last_winner_<crate>%` | Who last won from the crate. |
| `%uxmcrates_last_reward_<crate>%` | What they won. |
| `%uxmcrates_chance_<crate>_<reward>%` | The player's real chance of that reward as a percentage, with `odds.decimals` places. Empty when `odds.show` is off. |

### The board of crates opened

The board answers the grammar every UXM plugin's boards share, so a hologram written for another plugin's board
works here. The board is `opened`; `<period>` is `alltime`, `monthly`, `weekly` or `daily`. Every opening
counts, one at a time or in bulk; the all time board starts from the openings counted before it existed.

| Placeholder | Answer |
|---|---|
| `%uxmcrates_top_opened_<period>_<n>_name%` | The name at place n, or the word for nobody. |
| `%uxmcrates_top_opened_<period>_<n>_value%` | How many crates the player at place n opened, or the word for none. |
| `%uxmcrates_top_opened_<period>_<n>_head%` | The player at place n for a `<head:...>` tag, or `leaderboard.empty-head`. |
| `%uxmcrates_rank_opened_<period>%` | The reader's own place, or the word for none. |
| `%uxmcrates_value_opened_<period>%` | How many crates the reader opened in the period. |
| `%uxmcrates_resets_<period>%` | The time until the period starts again, such as `4d 12h`, or the word for none for all time. |
| `%uxmcrates_head%` | The reader's own head, for a `<head:...>` tag. |

The places are read every `leaderboard.refresh-seconds`; `leaderboard` in `config.conf` also sets the size, the
periods, the time zone, the first day of the week and how many past windows are kept. The words come from
`placeholder.*` in the messages files.

An unknown placeholder is left as written, which tells an operator they wrote one the plugin does not have.
