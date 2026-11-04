---
title: Menus as forms
order: 1303
description: Chest menus, sign editors and anvils of other plugins as Bedrock forms.
icon: layout-list
---

| Switch | Screen | Becomes |
|---|---|---|
| `menu-forms` | A chest, hopper, dispenser or dropper menu another plugin opens | A form: one button per item, the item's name as the label, the first line of its lore under it, its picture as the icon |
| `sign-forms` | A sign editor a plugin opens | A form with four lines |
| `anvil-forms` | An anvil a plugin opens for typing | A form with a text box |

A button sends the click the plugin waits for, so the plugin's own code runs, and the form comes back when the menu
stays open. Panes without a name are left out. Menus with a block or an entity behind them (a real chest, a
villager) are not touched, and neither are menus of plugins built on uxmLib, which have forms of their own.

## Who gets a form

1. A player who chose chests (`/menus chest` or the settings form) always gets the chest.
2. Otherwise the most specific rule of [menus.conf](../config/menus-conf.md) decides: a rule naming the plugin and a
   title wins over one naming only the plugin, which wins over one for every plugin.
3. With no rule, `default` decides.
4. With `touch = form`, a menu whose title carries a picture opens as a form for a touch player even where the rules
   say chest, since a touch player never gets the picture.

`/uxmbedrock menus` lists the rules. The plugin a menu belongs to is the first plugin on the call stack that opened
it, by the name `/plugins` shows.

Java dialogs are turned into Bedrock forms by Geyser itself.
