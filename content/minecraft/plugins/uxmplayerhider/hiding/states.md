---
title: States
order: 301
description: What each state shows, who counts as important, and how the view is kept right as players come and go.
icon: layers
---

| State | Shows | Name in a sentence (`state.<state>.name`) |
|---|---|---|
| `all` | Every player | `everybody`, in the `good` colour |
| `important` | Players who hold `uxmplayerhider.important` | `only the people who matter`, in the `warn` colour |
| `none` | Nobody | `nobody`, in the `bad` colour |

The ids `all`, `important` and `none` are what `/hide state` takes, what the database stores, and what
`%uxmplayerhider_state%` renders. They do not change with the language.

## The order

The tool and the bare `/hide` walk `all`, `important`, `none`, and wrap to `all`. The order is fixed.

## Important

A player is important to everybody when they hold `uxmplayerhider.important`, read at the moment a view is applied.
It is `op` by default. The shipped words speak of "staff and friends": the plugin reads no friends list, so grant
the node to the players you mean, or change the words in the messages files.

A node granted or taken while a player is online takes effect for a viewer the next time that viewer's view is
applied: their next change, join, respawn or world change.

## What hidden means

A hidden player is taken out of the viewer's world and tab list by the server, as a vanish does. The hidden player
is not told and still sees the viewer. Chat is untouched: a hidden player's messages still arrive.

## Keeping the view right

Hiding is held by the server per pair of players and forgotten when either leaves, so the plugin applies it again
whenever it could be out of date:

| When | What is applied |
|---|---|
| A player joins or respawns, one tick after | Their view of everybody, and everybody's view of them |
| A player changes world | The same |
| A player changes state | Their view of everybody |
| The state changes on another server of the network | That player's view of everybody, on this server |
| The plugin starts on a running server | Every online player, in both directions |

So a player who joins after somebody hid everybody is hidden from that somebody at once.

Related: [The tool](tool.md), [Permissions](../permissions/reference.md)
