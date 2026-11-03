---
title: Overview
order: 601
description: The four windows, their files, what opens each, and the shape every tile shares.
icon: layout-dashboard
---

| Window | File | Rows | Opened by |
|---|---|---|---|
| [Wardrobe](wardrobe.md) | `wardrobe.conf` | 5 | `/cosmetics`, the opener |
| [Family](category.md) | `category.conf` | 6 | A family tile, `/cosmetics open <family>` |
| [Mystery boxes](boxes.md) | `boxes.conf` | 3 | `/cosmetics boxes`, a station of every box |
| [Roulette](roulette.md) | `roulette.conf` | 3 | A box tile, a station of one box |

Every tile draws the house tooltip: a title, a crumb under it, a description, the details, and what a click does. The
words are in the `menu` block of the [messages files](../config/messages.md); the layout is in the window file. See
[Menu files](../config/menu-files.md).

A button plays `menu.sounds.click`, a page arrow `menu.sounds.page` and a refused click `menu.sounds.denied`, from
[config.conf](../config/config-conf.md#menu). A Bedrock player with Geyser or Floodgate gets each window as a form.

Related: [Menu files](../config/menu-files.md)
