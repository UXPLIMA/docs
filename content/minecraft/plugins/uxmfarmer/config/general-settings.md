---
title: general-settings.yml
order: 52
description: Prices, limits, collection rules, members and notifications.
icon: sliders-horizontal
---

This file is the economy and the rules. `config.yml` decides which plugins uxmFarmer talks to;
this one decides what a farmer costs and what a player is allowed to do with it.

## Prices and limits

```yaml
maximum-farmer-per-player: 2
farmer-price: 15000.0
tax-price: 0.25
return-price-on-delete: true
return-percent: 75
```

| Key | Meaning |
|---|---|
| `maximum-farmer-per-player` | Raised per player with `uxmfarmer.maxfarmer.<amount>` |
| `farmer-price` | Charged by `/farmer buy` |
| `tax-price` | Default tax on sales, as a fraction (`0.25` is 25%) |
| `return-price-on-delete` | Refunds the full price when a farmer is deleted |
| `return-percent` | Percentage refunded by `/farmer return` |

`tax-price` here is the floor; a product or a level may override it, and
`uxmfarmer.taxdiscount.<percentage>` reduces whatever applies. Tax is the main sink in this plugin (it is where the money a farmer generates goes back out of the economy)
so tune it before you tune
prices.

## The commands players get

```yaml
farmer-command-opens-gui: false
farmer-command-opens-buy-menu: true
disable-buy-command: false
sell-all-requires-confirmation: true
where-players-can-reach-farmer: NEAR_FARMER
```

`where-players-can-reach-farmer` has two values:

- `NEAR_FARMER`: the player must be standing near their farmer.
- `EVERYWHERE`: the player can use it from anywhere on their island.

`EVERYWHERE` only works with island hooks. Every other region hook behaves as `NEAR_FARMER`
regardless of what you set here.

`disable-buy-command: true` removes purchasing entirely, leaving farmer eggs as the only source,
which is how you sell farmers through a crate or a store instead of a command.

## Collection rules

```yaml
disabled-products-grow-physically: false
collect-dropped-items: true
collect-player-dropped-items: true
collect-player-dropped-items-by-default: true
collect-custom-items: false
```

`collect-custom-items` means items with a custom name, custom lore or any enchantment. It is off by
default and should stay off: a custom item has no price and no place in a product storage, and
collecting one is how a player loses it.

`disabled-products-grow-physically: true` makes switched-off crops grow as vanilla crops. It costs
real performance on an island world and is documented in the file itself as not recommended.

## Upgrades

```yaml
upgrade-all-upgrades-everything: false
upgrade-all-must-meet-all-requirements: false
upgrade-all-discount: 50
only-owners-can-upgrade: false
```

Covered on [Storage and Levels](../features/storage-and-levels.md#upgrade-all).

## Members

```yaml
adding-member-requires-permission: false
add-new-region-members-to-farmer: true
remove-leaving-region-members-from-farmer: true
transfer-farmer-ownership-to-region-owner-on-leave: true
disable-transfer: false
```

Covered on [Members and Ranks](../features/members-and-ranks.md).

## Notifications

```yaml
glow-if-stock-is-full: true
notify-when-stock-gets-full:
  enabled: true
  notify-members: true
  notify-amount: 3
```

`notify-amount` caps how many times a player is told about the same full product, so a farm that
stays full does not become a chat spammer.

## Placement

```yaml
place-farmer-on-region-creation: false
first-location:
  enabled: false
  modify-by: -5.5, 1.5, 3.5
  rotation: -88, 0.8
```

With `place-farmer-on-region-creation: true`, every new island or claim gets a farmer immediately.
`first-location` then decides where it stands, as an offset from the region centre and a pitch and
yaw. Tune it against your island schematic: the default offset assumes a particular layout and will
put the farmer inside a wall on most others.

<Callout type="note" title="Region hooks and automatic placement">

GriefDefender does not support `place-farmer-on-region-creation` at all. ProtectionStones supports
it but the plugin recommends leaving it off there.

</Callout>

## Input

```yaml
add-member:
  required-online: true
  input-type: SIGN
  chat-timeout: 15

take-amount:
  input-type: SIGN
  chat-timeout: 8
```

`SIGN` opens a sign editor; `CHAT` waits for the player's next message for `chat-timeout` seconds.
Sign input is more reliable when other plugins are handling chat.

## Logs and defaults

```yaml
disable-logs: false
farmer-visible-by-default: true
default-click-actions:
  - "[menu] main"
  - "[sound] ENTITY_ALLAY_AMBIENT_WITHOUT_ITEM"
```
