---
title: Island Type (/is create)
order: 590
description: 'Running /is create with no type opens the Island Type menu: a one-click
  picker for the starter island you want. Choosing a type creates the island
  immediately.'
icon: mountain
---

---

## What It Shows

Every entry under `schematics:` in [config.yml](../configuration/config-yml.md#schematics)
becomes its own item, using that type's `display-name` and `icon`. The list is
paginated automatically, so adding more island types never outgrows the menu.

Clicking an item runs `create` with that type, so the island is built from the
matching `.schem` and you're teleported straight to it.

<Callout type="tip" title="Skip the menu">

Players who already know what they want can pass the type directly:
`/is create desert`. The menu only opens for a bare `/is create`.

</Callout>

<Callout type="note" title="When the menu is skipped">

If FastAsyncWorldEdit isn't installed (so schematics can't be pasted), a bare
`/is create` just builds the default starter platform instead of opening the
menu.

</Callout>

---

## Customizing

The menu type is `schematics`, which triggers automatic population. Edit the
title, the slots the types fill (`head-slots`), and the per-type name/lore
(`schematic-name` / `schematic-lore`, with `{name}` and `{key}` placeholders) in
`menus/create.yml`. Per-type icons and display names live in `config.yml`. See
[Menu Customization](../configuration/menus.md).
