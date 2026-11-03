---
title: Overview
order: 401
description: The four windows, what opens each one, Bedrock players, and the click sounds.
icon: mouse-pointer-click
---

uxmEasterEggs has four windows. Players see two; operators use the other two. Every window is a file in `menus/`:
see [Menu files](../config/menu-files.md) to change one. The screenshots on these pages are taken on a lobby whose
collection, leaderboard and picker files were cut to three rows; the tables describe the shipped files.

| Command | Window | File | Permission |
|---|---|---|---|
| `/eggs` | [Collection](collection.md) | `menus/collection.conf` | `uxmeastereggs.use` |
| `/eggs top` | [Leaderboard](leaderboard.md) | `menus/leaderboard.conf` | `uxmeastereggs.use` |
| `/uxmeastereggs editor` | [Placed eggs](placed-eggs.md) | `menus/admin.conf` | `uxmeastereggs.admin` and `uxmeastereggs.admin.editor` |
| `/uxmeastereggs create` with no type | [Type picker](picker.md) | `menus/picker.conf` | `uxmeastereggs.admin` and `uxmeastereggs.admin.create` |

A window whose file you delete stops opening at the next reload, and the player reads `menu.missing`. The next start
writes the shipped file back.

## Bedrock players

A player who joins through Geyser or Floodgate gets the same windows as Bedrock forms: a button per tile, in the same
order. Nothing needs setting up.

## Sounds

Every window plays the book page sound when it opens, from its own `open-actions`. The buttons of the operator windows
play their own note block sounds. The shared click, page-turn and refusal sounds are under `menu.sounds` in
[`config.conf`](../config/config-conf.md#menu), read again on reload.

## Conventions

- **A glowing tile is a finished one**: a type whose every egg the player has found.
- **Words come from the messages files.** Every title, name and tooltip is a line of the
  [messages files](../config/messages.md) under `menu`, in the viewer's language.
- **Heads cost no network call.** A type's head is drawn from a skin URL or a base64 texture; a player's head on the
  board from their account id. A player name is never looked up.

Related: [Menu files](../config/menu-files.md), [Collection](collection.md), [Placed eggs](placed-eggs.md)
