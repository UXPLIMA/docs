---
title: Main Menu (/is)
order: 580
description: 'Opening /is brings up the main menu: the hub for everything an island
  owner does. Every slot, icon, name, and action is editable in menus/main.yml; this
  page describes the default layout.'
---

---

## Layout

The main menu is a `warp`-type menu: the center is filled with the **heads of
players who are currently online**, and clicking a head **visits** that island
(across servers when the proxy is on). Around the edges sit the action buttons.

| Button | Icon | Action |
|--------|------|--------|
| **Island Warps** | Ender Pearl | Open the [Warp menu](warps.md) |
| **Help & Commands** | Book | Open the help menu |
| **Go To My Island** | Grass Block | Teleport home |
| **Create Island** | Nether Star | Open the [Island Type menu](island-type.md) to pick and create an island |
| **Settings** | Comparator | Open the [Settings menu](settings.md) |
| **Upgrades** | Experience Bottle | Open the [Upgrades menu](upgrades.md) |
| **Delete Island** | TNT | Open the delete confirmation |
| **Previous / Next Page** | Arrow | Page through online islands |
| **Close** | Barrier | Close the menu |

---

## The Island Heads

Each head represents an online player's island. By default it shows:

- The owner's rank (via `%vault_group%`) and name
- Island **level** and **points**
- **Members** vs. team limit
- Visitor lock state
- A per-member list (`{member_list}`)

Clicking a head runs a visit to that island.

<Callout type="tip" title="Customizing">

Change which slots hold heads (`head-slots`), the head name/lore, and the
member line format right in `menus/main.yml`. See
[Menu Customization](../configuration/menus.md).

</Callout>
