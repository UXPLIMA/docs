---
title: The waiting room
order: 401
description: Where a player waits before logging in, what they may not do, timeouts, the dialogs, chat input and the pin pad.
icon: door-closed
---

## Where they wait

`waiting-room` in [screens.conf](../config/screens-conf.md):

| Room | The player |
|---|---|
| `dialog` | Reads a dialog before the world, while the client is still connecting; never stands in the world unchecked. Clients older than 1.21.6 fall back to `frozen` |
| `frozen` | Stands in the world, held still, with the commands and the dialogs there |
| `login-world` | Stands in an empty world of the plugin's own, `login-world-name` (`uxmauth_login`), with one block to stand on, and is moved back after the login. Folia cannot make a world while it runs: make it yourself under that name, or players wait frozen |

The dialog comes before the player exists, so a permission plugin cannot be asked yet: there, `uxmauth.login` and
`uxmauth.register` count only when the server grants them to everyone by default. To deny one through a permission
plugin, use `frozen`.

| Key | Default | Meaning |
|---|---|---|
| `cancel` | `"kick"` | What closing the dialog does: `kick`, or `world` (into the frozen world, where the commands still work) |
| `can-close` | `false` | Whether Escape closes the dialog |
| `chat-input` | `false` | A player in the world types the password or code as plain chat; the line reaches nobody |
| `pin-pad` | `false` | A ten-key pad instead of a typed password; pair it with `passwords.allowed = "[0-9]+"` |
| `rules.enabled`, `rules.lines` | off | Rules a player must accept before they register |

The screens are register, login, replace (a right password that no longer meets the rules), captcha, code, recover and
pin; see [Screens](../menus/screens.md).

## What a waiting player may not do

`hold` in [config.conf](../config/config-conf.md#hold): each switch is its own, and turning one off lets a player who
never logs in do that thing.

| Switch | Holds |
|---|---|
| `movement`, `chat`, `commands` | Moving, chatting, commands; `allowed-commands` lists others they may still run |
| `blocks`, `interact`, `inventory`, `damage` | Breaking and placing, using things, the inventory, taking or dealing damage |
| `vehicles`, `portals`, `teleports` | |
| `hide-players` | Hides them from everyone, and everyone from them |
| `blindness` | Blinds them |
| `hide-inventory` | Hides their inventory until they log in, and gives it back after |
| `hide-stats` | Hides health, food, flight, speed and operator status |
| `join-message-waits` | Holds the join message until the login; no quit message for one who never logged in |

## Where they stand

`location.login-spot` places a frozen player; empty, they wait where they joined. After the login every player goes back
to where they stood. `location.rescue` moves a player who joins inside a solid block or a portal to the nearest safe spot
up to `rescue-reach` (6) blocks above, or to the world's spawn.

## Timeouts

| Key under `timeouts` | Default |
|---|---|
| `register-seconds` | `90` |
| `login-seconds` | `60` |
| `reminder-seconds` | `5`: how often the reminder repeats |

## One connection per name

A second connection under a name already online or waiting is refused. `names.reconnect-from-same-address = true` lets
it in from the address the first one holds, sending the first one away, for a player whose first connection hung.

Related: [screens.conf](../config/screens-conf.md), [Effects](effects.md)
