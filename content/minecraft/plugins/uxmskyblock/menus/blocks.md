---
title: Block Values
order: 630
icon: blocks
---

`/is block` opens a paginated menu showing **what every block is worth** in
points. It's the in-game version of [block-values.yml](../configuration/block-values-yml.md),
so players can see exactly how to level up without leaving the game.

---

## What It Shows

Each block listed in `block-values.yml` gets its own item, displaying its point
value. The menu is paginated automatically when there are many blocks. The display
name and lore for each entry are configurable via the `block-name` / `block-lore`
fields in `menus/blocks.yml`.

---

## Why It Matters

Placing a block **adds** its points to the island; breaking it **removes** them.
This menu tells players which blocks give the most points per slot, so they can
plan their build to climb the [leaderboard](../commands/leaderboard.md).

---

## Customizing

The menu type is `blocks`, which triggers automatic population. Edit titles,
paging, and the per-block name/lore in `menus/blocks.yml`. See
[Menu Customization](../configuration/menus.md).
