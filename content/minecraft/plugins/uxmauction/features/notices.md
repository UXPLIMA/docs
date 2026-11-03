---
title: Notices
order: 310
description: Effects for every moment, the Discord webhook, and events for other plugins.
icon: bell
---

## Effects

`effects` in [config.conf](../config/config-conf.md#effects) holds a list of action lines for each of nine moments:

| Moment | When | Shipped |
|---|---|---|
| `listed` | A listing went up | `@line` |
| `listed-and-sold` | A listing went up and an order took it on the spot | `@line`, a level-up sound |
| `bought` | A player bought off the shelf | `@line` |
| `bid-leading` | A bid was taken and leads | `@line` |
| `bid-outbid` | A bid was taken but another was higher | `@line` |
| `bid-beaten` | Somebody bid over this player | `@line` |
| `auction-sold` | This player's auction ended and sold | `@line`, a level-up sound |
| `auction-won` | This player won an auction | `@line`, a higher level-up sound |
| `mailbox-took` | Something was taken out of the mailbox | `@line` |

`@line` is the line the house chose for this outcome: a listing that charged a fee and one that did not are the same
moment with two lines, and `@line` is the right one every time. `@some.key` is any other line of the messages files, and
anything else is written as you wrote it.

Verbs: `[message]`, `[broadcast]`, `[title]`, `[subtitle] <title> | <subtitle> <in> <stay> <out>`, `[actionbar]`,
`[sound] <key> <volume> <pitch>`, `[particle] <name> <count> <spread> [data]`, `[console]`, `[player]`. Delete a line to
keep the shipped one; write `[]` for silence. A refusal has no list: nothing happened in the world, so it stays one line
in the messages files.

## The Discord webhook

One message for a new listing, one for a finished sale, one for every move an operator makes on somebody else's row.

| Key under `webhook` | Default | Meaning |
|---|---|---|
| `enabled` | `false` | Off until you write an address |
| `url` | `""` | The webhook address. Never write it into a file that reaches version control |
| `timeout` | `"5s"` | How long the plugin waits; it never waits on the thread that sold the item |
| `listing` | `A new listing: {amount} x {item} at {price} by {seller}` | Also `{total}`, `{kind}` |
| `sale` | `Sold: {amount} x {item} for {total} to {buyer}` | Also `{seller}` |
| `admin` | `{who} used {move} {about}` | |
| `least.listing.<currency>`, `least.sale.<currency>` | none | The least an event has to be worth before it is posted; operator moves are always posted |

## Events for other plugins

With `events.enabled = true` the plugin publishes a Bukkit event for a new listing, a finished sale and every operator
move, on a tick thread so a listener may use the server API. Nothing is published to a server that has no listener.

Related: [config.conf](../config/config-conf.md), [Messages](../config/messages.md)
