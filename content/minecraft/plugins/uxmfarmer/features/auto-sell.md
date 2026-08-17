---
title: Auto Sell
order: 34
icon: banknote
---

Auto Sell empties a product's storage the moment it fills, paying the owner. It is the module that
lets a player stop visiting their farm, and therefore the one whose price and tax settings decide
how much money your server prints.

```yaml
auto-sell:
  name: "Automatic Sell"
  status: true
  default-status: false
  permission-required: true
  send-message: false
  send-to-members: false
  everything: true
  items:
    - WHEAT
    - CARROT
```

| Option | What it does |
|---|---|
| `status` | Whether the module exists on this server |
| `default-status` | Whether a new farmer has it switched on |
| `permission-required` | Requires the owner to hold `uxmfarmer.autosell` |
| `send-message` | Messages the owner on each sale |
| `send-to-members` | Sends that message to active members as well |
| `everything` | Sells every collected product rather than only the listed ones |

## The items list, and what `everything` does to it

The `items` list has two different meanings depending on `everything`:

- `everything: false` — the farmer auto-sells **only** the listed materials.
- `everything: true` — the farmer auto-sells everything it collects, and the list becomes the set
  of products a player is **allowed to toggle** in their own auto-sell menu.

The second reading is the one that surprises people. With the default configuration, every product
is sold automatically and a player may only opt WHEAT and CARROT out of it.

## Money and tax

The sale price comes from your [price source](../integrations.md#price-source-hook) — either
`unit-price` in `collected-materials.yml` or a live price from your shop plugin. The tax comes from,
in order of precedence:

1. `tax-price` on the product's current level
2. `tax-price` on the product
3. `tax-price` in `general-settings.yml` (0.25 by default)

Then `uxmfarmer.taxdiscount.<percentage>` reduces the result for that player. A player with
`uxmfarmer.taxdiscount.50` pays half the tax the level asked for.

## Messages

`send-message` is off by default, and it should usually stay off: a farm at full tilt sells
continuously, and the chat becomes unreadable. `notify-when-stock-gets-full` in
`general-settings.yml` is the message players actually want, and it only fires when something has
gone wrong.

<Callout type="note" title="Auto Sell needs somewhere to sell to">

If your price source is a shop plugin and a product is not in the shop, the farmer has no price for
it, so it is not sold and the storage fills. Set `price-source-hook: default` and price everything
in `collected-materials.yml` if you would rather never have that gap.

</Callout>
