---
title: Permissions
order: 270
description: Every node, what it grants, and what to give your default group.
icon: key-round
---

There are only four families of node, and one of them is configurable per trade.

## Slots

| Node | Grants |
|---|---|
| `uxmblacksmith.slot.1` … `uxmblacksmith.slot.10` | One concurrent trade each |

A player's slot count is **how many of the ten they hold**, not the highest. `uxmblacksmith.slot.11`
does nothing. The mapping from node to inventory position lives in `gui.yml`, so you can point a
slot at a node of your own naming.

<Callout type="warning" title="Grant uxmblacksmith.slot.1 before launch">

Nothing is granted by default. Without it a player opens the menu, sees the trades, and every click
refuses.

</Callout>

## Burn

| Node | Grants |
|---|---|
| `uxmblacksmith.burn.1` | +5% success chance (a multiplier, not points) |
| `uxmblacksmith.burn.2` | +10% success chance |
| `uxmblacksmith.burn.bypass` | Never burn — on trades using the default `burnBypassPermission` |
| `uxmblacksmith.bypassall` | Never burn, on **every** trade |

Multipliers do not stack; the largest held wins. Both node names and their values come from
`burn.permissionBasedMultipliers`, so you can add a `.3` at 20 or rename them to match your ranks.

`burnBypassPermission` is set per trade and defaults to `uxmblacksmith.burn.bypass`. Give different
tiers different bypasses by setting it per recipe.

<Callout type="danger" title="uxmblacksmith.bypassall removes risk from the whole economy">

It is the global bypass, not a staff convenience. On a donor rank it makes every legendary recipe a
guaranteed craft. Sell `uxmblacksmith.burn.1` and `.2` instead.

</Callout>

## Admin

| Node | Grants |
|---|---|
| `uxmblacksmith.admin` | Every admin subcommand, the editor, and 99 slots |

One node for everything: `reload`, `giveboost`, `editor`, `namedebug`, `binary`, and the whole
`admin` tree — progression edits, queue intervention, analytics and export.

It also treats the holder as having **99 slots**, which is why `%uxmblacksmith_maxslots%` returns 99
for staff. That is a convenience for testing, not a bug.

There is no read-only staff node. Anyone who can see the analytics can also set a player's level.

## Categories

Set on the category, so the node is yours to name:

```yaml
categories:
  legendary:
    permission: 'uxmblacksmith.category.legendary'
```

Empty or unset means everyone. A player without it does not see the category at all.

## Trades

A trade has no permission of its own. Gate one with a `PLACEHOLDER` requirement instead:

```yaml
requirements:
  1:
    displayName: '&eSpecial Permission Required'
    material: PLACEHOLDER
    condition: '%permission_has_blacksmith.special%'
    requiredOutput: 'yes'
```

This is better than a real permission node would be — the player sees *why* the trade is locked
rather than finding it missing.

## A worked setup

```yaml
default:
  - uxmblacksmith.slot.1

vip:
  - uxmblacksmith.slot.2
  - uxmblacksmith.burn.1

mvip:
  - uxmblacksmith.slot.3
  - uxmblacksmith.slot.4
  - uxmblacksmith.burn.2
  - uxmblacksmith.category.legendary

staff:
  - uxmblacksmith.admin
```

Note what is **not** here: `uxmblacksmith.bypassall` on any paid rank. Slots and better odds scale
cleanly; removing risk entirely does not.

Slots five to ten are left for the `artisan_slots` perk to unlock, so progression has somewhere to
go for players who did not buy a rank.
