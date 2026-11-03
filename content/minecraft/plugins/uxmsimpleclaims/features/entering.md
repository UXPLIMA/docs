---
title: Entering and teleports
order: 309
description: What a player sees walking in and out, and what other plugins' teleports may do.
icon: log-in
---

## Entering and leaving

| Key in `display` | Default | Shows |
|---|---|---|
| `title` | true | A title |
| `action-bar` | true | An action bar line |
| `chat` | false | A chat line |
| `boss-bar` | false | A boss bar while inside |
| `greeting` | true | The owner's greeting and farewell, in chat |
| `border` | `particles` | How edges are drawn: `particles` or `blocks` |
| `border-seconds` | 10 | How long |

Turn off what your server shows another way.

## Teleports

`teleport.policy` decides whether another plugin's teleport (a home, a warp, `/tpa`) may bring a player into a claim:

| Policy | Arrives |
|---|---|
| `strict` | Only players the claim lets teleport |
| `consent` | As strict, unless they teleport to a member of the claim. The default |
| `free` | Anybody |

Related: [config.conf](../config/config-conf.md#display-and-teleport)
