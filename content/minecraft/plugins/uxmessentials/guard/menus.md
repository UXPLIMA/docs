---
title: Menus
order: 1110
description: The eight guard windows, what every slot and click does, and how to restyle them.
icon: layout-grid
---

Contents: [Opening](#opening) · [Region list](#region-list) · [Region](#region) · [Rules](#rules) ·
[Roles](#roles) · [Behaviours](#behaviours) · [Templates](#templates) · [History](#history) ·
[Regions inside](#regions-inside) · [Restyling](#restyling)

## Opening

`/guard menu`, or `/guard` alone. Staff with `uxmessentials.guard.menu` get the [region list](#region-list).
A region owner or manager without it gets the [roles](#roles) window of the region they stand in, where they
hand out `member` and `guest`. Every edit made in a window is the same edit as the command, with the same
permission check, history entry and message, and the window redraws with the new value.

Bedrock players get every window as a native form, built from the same files.

All windows are six rows. The tiles sit in rows two to five inside a grey frame; the bottom row holds the
previous page arrow (slot 48), back (49) and next page arrow (50).

## Region list

The regions of the world you stand in, highest priority first.

| Slot | Shows | Left click | Right click |
|---|---|---|---|
| 10 to 43 | A region, with its shape, priority, rule and role counts | Opens the [region](#region) | Draws its borders for you |
| 48, 50 | Previous and next page | Turns the page | |

## Region

One region. Each tile shows its current value in the lore.

| Slot | Tile | Click |
|---|---|---|
| 20 | Rules | Opens the [rule editor](#rules) |
| 21 | Roles | Opens the [roles](#roles) |
| 22 | Behaviours | Opens the [behaviours](#behaviours) |
| 23 | Templates | Opens the [template picker](#templates) |
| 24 | History | Opens the [history](#history) |
| 29 | Regions inside | Opens the [regions inside this one](#regions-inside) |
| 30 | Borders | Draws the borders for you |
| 31 | Priority | Asks for a number |
| 32 | Tags | Left click asks for a tag to add, right click for a tag to remove |
| 33 | Lock | Locks or unlocks the region (iron bars when locked) |
| 40 | Remove | Asks to confirm, then removes the region |
| 49 | Back | The region list |

## Rules

The actions at one level of the [action tree](actions.md), each showing its state for one target.

| Slot | Shows | Left click | Right click |
|---|---|---|---|
| 10 to 43 | An action and its rule for the chosen target: unset, allow or deny | Steps unset, allow, deny, unset | Opens the actions under it |
| 46 | The target the rules are for | Steps through `everyone`, `nonmember` and the roles | Asks for a `perm:` or `cond:` target |
| 49 | Back | One level up, or the region | |

## Roles

Every player with a role in the region.

| Slot | Shows | Left click | Right click |
|---|---|---|---|
| 10 to 43 | A player and their role | Moves them to the next role | Takes the role away |
| 46 | Add a member | Asks for a name, and makes that player a member | |
| 49 | Back | The region (for an owner or manager, closes) | |

An owner or manager here can only hand out and take back `member` and `guest`.

## Behaviours

Every behaviour guard knows, with its value in this region.

| Slot | Shows | Left click | Right click |
|---|---|---|---|
| 10 to 43 | A behaviour and its value | Asks for a value in chat; a wrong value is refused with the reason | Clears it |
| 49 | Back | The region | |

## Templates

The templates from `modules/guard/config.conf`.

| Slot | Shows | Left click |
|---|---|---|
| 10 to 43 | A template, with how many rules and behaviours it has, marked when linked | Links the region to it |
| 46 | Unlink | Removes the link |
| 49 | Back | The region |

## History

The last changes to the region, newest first.

| Slot | Shows | Left click |
|---|---|---|
| 10 to 43 | A change: its id, who made it, when, and what changed | Undoes it; the undo is recorded too |
| 49 | Back | The region |

## Regions inside

The regions whose parent is this one, as tiles like the region list: a left click opens one, a right click draws
its borders. Back returns to the parent region.

## Restyling

The windows are the files in `plugins/uxmEssentials/modules/guard/gui/`:

| File | Window |
|---|---|
| `guard-regions.conf` | Region list |
| `guard-region.conf` | Region |
| `guard-rules.conf` | Rules |
| `guard-roles.conf` | Roles |
| `guard-behaviours.conf` | Behaviours |
| `guard-templates.conf` | Templates |
| `guard-history.conf` | History |
| `guard-children.conf` | Regions inside |

They use the [menu engine](../menus/engine.md): move slots, change materials, sounds and the frame, and
`/guard reload` picks the change up. The words come from the language files (`guard.gui.*` keys), so a window
reads in each player's language; change them in `messages/`.
