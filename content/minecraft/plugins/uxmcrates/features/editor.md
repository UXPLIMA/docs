---
title: The editor
order: 410
description: Building crates, rewards and keys in windows, what it writes, and turning it off.
icon: pencil
---

`/uxmcrates editor` (`uxmcrates.admin.editor`) opens a window to build crates, rewards and keys without leaving the
game. It writes the same files an operator edits by hand, keeps their comments, and never writes `config.conf` or a
window file. A change is live at once.

## What it reaches

| Window | Does |
|---|---|
| Home | Two buttons: the crates and the keys |
| Crates | A tile per crate. Left click edits its settings; right click builds its rewards. The book at the bottom left makes a new crate from `editor/new-crate.conf` |
| A crate's settings | Every block of the crate file: name, key, keys per opening, key kind, shape and material, animation, cooldown, permission, idle effect, no-repeat, visibility and radius, broadcast and threshold, cost, requirements, open and win effects, bulk cap and announce, pick crate settings, the world rule. The barrier at the bottom right deletes it after a confirmation |
| A crate's rewards | A tile per reward with its chance and what it gives. The last slot makes a reward of the item in your hand |
| A reward | The item (hold one and click), name, lore, weight, rarity, permission, announce, commands, money, player and global limits, icon, guarantee mode and opening, linked rewards, fallback, luck. Delete with a confirmation; the last reward of a crate cannot be deleted |
| Keys | A tile per key; a book makes a new key from `editor/new-key.conf` |
| A key | Name, material, amount, glint, lore, tradeable, crafting, shapeless, craft amount, craft permission, the nine squares, window order, hidden, store lines, left and right click |

A toggle steps through its values. A text value is typed when asked. A list opens a window of lines: shift left click
writes one again, shift right click takes it out, and the last slot adds one.

A reward made from the item in your hand keeps everything on it: enchantments, trims, potions, a head's owner, a
written book.

## Ids

A new crate or key asks for an id: lower case letters, digits, a dash or an underscore (`editor.bad-id`). An id that
exists answers `editor.exists` or `editor.key-there`.

## Turning it off

`editor.enabled = false` in [config.conf](../config/config-conf.md#editor) is for a server that keeps its content in
version control. The editor's windows are not registered, no chat or anvil input is listened for, and the command
answers `editor.off`.

Related: [Editor windows](../menus/editor.md), [Crate files](../config/crate-files.md)
