---
title: Emoji
order: 307
description: Shortcuts that become glyphs, the shipped emoji, the window, and per-emoji permissions.
icon: smile
---

Module: `emojis`. A player types a shortcut and readers see the glyph. `:heart:` becomes a red heart.

| Emoji | Shortcuts | Glyph | Node |
|---|---|---|---|
| heart | `:heart:` | ❤ (red) | |
| smile | `:smile:`, `:)` | ☺ | |
| frown | `:frown:`, `:(` | ☹ | |
| star | `:star:` | ★ (yellow) | |
| sun | `:sun:` | ☀ (yellow) | |
| cloud | `:cloud:` | ☁ | |
| umbrella | `:umbrella:` | ☂ (aqua) | |
| check | `:check:`, `:yes:` | ✔ (green) | |
| cross | `:cross:`, `:no:` | ✘ (red) | |
| note | `:note:`, `:music:` | ♪ (light purple) | |
| skull | `:skull:` | ☠ | |
| swords | `:swords:`, `:fight:` | ⚔ (grey) | |
| coffee | `:coffee:` | ☕ (gold) | |
| snowman | `:snowman:` | ☃ | `uxmchat.emoji.seasonal` |
| flower | `:flower:` | ❀ (light purple) | |
| arrow | `:arrow:`, `->` | ➜ | |
| shrug | `:shrug:` | ¯\\_(ツ)_/¯ | |
| tableflip | `:tableflip:` | (╯°□°)╯︵ ┻━┻ | `uxmchat.emoji.faces` |

Every emoji needs `uxmchat.emoji` too. One line turns at most `per-line` (10) shortcuts into glyphs.

`/emoji` opens the [emoji window](../menus/emojis.md), which lists every emoji the player may use; a click closes it
and sends the shortcut as a line that copies itself.

A glyph is MiniMessage, so it can carry a colour. Use characters the game's own font draws, or a resource pack's.

Related: [emojis.conf](../config/emojis-conf.md)
