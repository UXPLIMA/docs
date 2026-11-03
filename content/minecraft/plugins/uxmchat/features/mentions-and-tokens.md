---
title: Mentions and tokens
order: 404
description: Mentioning players, @here and @everyone, how a mention is told, and the item, inventory, position, money and other tokens.
icon: at-sign
---

## Mentions

Module: `mentions`. `@Name` in a line draws in the `style` of `mentions` and tells the named player.

| Key | Default | Meaning |
|---|---|---|
| `style` | `<accent><name></accent>` | How a mention is drawn; `<name>` is the mention as written, with its `@` |
| `bare-names` | `false` | Whether a player's name without `@` mentions them too |
| `max-per-line` | `5` | The most players one line may mention |
| `cooldown-seconds` | `10` | How long one player waits before mentioning the same player again |
| `sound` | `minecraft:entity.experience_orb.pickup` | The sound a mentioned player hears |

Each player picks how a mention tells them in their [settings](../menus/settings.md): a sound, a line over the
hotbar, a title, a boss bar, or nothing. A player who turned mentions off, or ignores the speaker's mentions, is not
told. A newcomer may mention `newcomer.mentions` (1) player per line.

| Mention | Reaches | Needs |
|---|---|---|
| `@here` | Everyone who hears the channel | `uxmchat.mention.here`, and `allow-here` on the channel |
| `@everyone` | Everyone online | `uxmchat.mention.everyone`, and `allow-everyone` on the channel |

## Tokens

A player writes a token in square brackets, and readers see it drawn. Each kind can be turned off, given a permission,
and answer to more than one name, under `tokens.kinds` in [config.conf](../config/config-conf.md#tokens-and-snapshots).

| Token | Names | Shows | Node |
|---|---|---|---|
| item | `[item]`, `[hand]`, `[i]` | The held item, with its tooltip | `uxmchat.token.item` |
| ping | `[ping]` | Your ping | none |
| pos | `[pos]`, `[loc]` | Your position | `uxmchat.token.pos` |
| time | `[time]` | The time of day, in `time-zone` | none |
| gametime | `[gametime]` | The time in your world | none |
| money | `[money]`, `[balance]` | Your balance, through Vault | `uxmchat.token.money` |
| inv | `[inv]`, `[inventory]` | A snapshot of your inventory | `uxmchat.token.inv` |
| ender | `[ender]`, `[ec]` | A snapshot of your ender chest | `uxmchat.token.ender` |
| shulker | `[shulker]` | A snapshot of the held shulker box | `uxmchat.token.shulker` |

One line draws at most `chat.tokens-per-line` (8) tokens.

**Clickable commands.** A player may write `[/spawn]`, and readers click it to run `/spawn`. Only the commands in
`tokens.commands` work, written in full with their slash: `/spawn`, `/rules`, `/discord` ship.

**Item guard.** Large items crash some clients when their tooltip is drawn. An item over `item-guard.max-bytes`
(8192), or of a kind in `item-guard.blocked` (`WRITTEN_BOOK`, `WRITABLE_BOOK`), is shown by its name with no tooltip.

## Snapshots

Module: `snapshots`. `[inv]`, `[ender]` and `[shulker]` become a link; a click opens a read-only copy of what was
inside at that moment, from any server of the network, for `snapshots.keep-minutes` (30). Node
`uxmchat.snapshot.view`. A Bedrock player reads it as a form, one line per stack.

## Quotes

Module: `quotes`. A quote button after each line types `/quote <line> ` for you; your answer goes out with a quote of
the line above it. Node `uxmchat.quote`.

Related: [config.conf](../config/config-conf.md), [Settings](../menus/settings.md)
