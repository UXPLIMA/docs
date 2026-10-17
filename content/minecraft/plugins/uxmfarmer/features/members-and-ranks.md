---
title: Members and Ranks
order: 37
description: Ranks, their permissions, adding members and following the region.
icon: users
---

A farmer has one owner and any number of members, and each member holds a rank. The rank decides
what they may do; the ranks themselves are yours to define.

## Ranks

Ranks live in `languages/<lang>/ranks.yml`. Three ship by default:

| Rank | Weight | Adds |
|---|---|---|
| Member | 100 | `SELL`, `COLLECT`, `TOGGLE_GLOWING`, `CHANGE_GLOWING_COLOR`, `SEE_LOGS`, `UPGRADE` |
| Leader | 500 | `CHANGE_SKIN_NAME`, `MOVE_HERE`, `CHANGE_APPEARANCE`, `SEE_MODULE_SETTINGS` |
| Owner | 1000 | `ACCESS_MANAGE_MENU` |

Each rank lists only what it *adds*. A rank holds everything granted by every rank of lower weight,
so a Leader can sell and upgrade without those permissions being repeated in its list.

```yaml
farmer-ranks:
  leader:
    id: 2
    name: 'Leader'
    weight: 500
    permissions:
      - CHANGE_SKIN_NAME
      - MOVE_HERE
      - CHANGE_APPEARANCE
      - SEE_MODULE_SETTINGS
    show:
      material: 'PLAYER_HEAD'
      display-name: '<#39a7fc>Role'
      head-data: 'texture-...'
      lore: [...]
      slots: [12]
```

`id` must be unique, `weight` sets the order, and `show` is the item drawn in the member management
menu, including the slot it occupies. Left-click promotes, right-click demotes, and the plugin
walks the ranks in weight order.

Adding a fourth rank is a matter of adding a block with a weight between two existing ones and
listing what it adds.

## Rank permissions

| Permission | What it allows |
|---|---|
| `ACCESS_MANAGE_MENU` | Open the manage menu (members, deletion), and switch the farmer's modules, collection and products |
| `SELL` | Sell products and XP from storage, and use conversions |
| `UPGRADE` | Buy product levels |
| `COLLECT` | Take products and XP out of storage |
| `TOGGLE_GLOWING` | Turn the farmer's glow on and off |
| `CHANGE_GLOWING_COLOR` | Change the glow colour |
| `CHANGE_SKIN_NAME` | Change the farmer's skin and name |
| `CHANGE_APPEARANCE` | Change entity type, profession, baby variant, and hide the farmer |
| `MOVE_HERE` | Move the farmer to their position |
| `SEE_LOGS` | Read the farmer's log |
| `SEE_MODULE_SETTINGS` | Open the module settings menus |

Each farmer can change which rank a permission needs, in its permission menu. A member who is not
the owner can only change a permission their own rank already holds, and cannot move it above
their own rank; only the owner can make a permission owner-only.

## Adding members

```yaml
add-member:
  required-online: true
  input-type: SIGN
  chat-timeout: 15

adding-member-requires-permission: false
```

`input-type` is `SIGN` or `CHAT`. Sign input is the safer choice: it cannot be intercepted by a chat
plugin and it does not leave the player in a state where their next message is swallowed. If you
use `CHAT`, `chat-timeout` is how long the plugin waits before giving up.

`adding-member-requires-permission: true` additionally requires `uxmfarmer.addmember`, which is how
you make member slots a rank perk.

## Members from the region

By default the farmer follows the region:

```yaml
add-new-region-members-to-farmer: true
remove-leaving-region-members-from-farmer: true
transfer-farmer-ownership-to-region-owner-on-leave: true
```

Someone trusted on the claim or added to the island becomes a farmer member automatically, and
loses it when they leave. The third setting handles the awkward case: a farmer owner who is not the
region owner and who leaves: the farmer goes to whoever owns the land rather than disappearing
with them.

Turn all three off if farmer membership should be a separate thing players manage themselves.

## Transferring and deleting

`disable-transfer: true` removes transfer entirely. Otherwise the owner, and only the owner, can
hand the farmer to a member through a confirmation menu.

`return-price-on-delete: true` refunds the farmer's price when a farmer is deleted; `/farmer return`
refunds `return-percent` of it (75% by default) which is the one players use. Both are covered on
[general-settings.yml](../config/general-settings.md#prices-and-limits).

## Logs

Sales, collections and member changes are logged, and anyone with `SEE_LOGS` reads them in the
logs menu. How many are kept is set in `config.yml`; see [Logs](../config/logs.md).
