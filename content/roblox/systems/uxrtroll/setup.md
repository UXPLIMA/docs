---
title: Setup
order: 1
description: Install, create fourteen products, protect your staff, and fire the first troll.
icon: play
---

Install with the standard four steps in
[Installing a system](../platform/installation.md): drag `uxrTrollSystem` into
**ServerScriptService** and press Play once.

## 1. Replace the product ids

`Configuration/Shared/TrollSettings.luau` ships with fourteen real developer product ids
that belong to somebody else.

<Callout type="danger" title="Every shipped id is somebody else's product">

A player who presses a button pays the original creator and gets nothing from you, and the
troll never lands, because the receipt arrives in a different game.

Create your own fourteen products before you do anything else.

</Callout>

## 2. Create fourteen products

In Creator Hub, under your game, create one developer product per button:

| Troll | Products |
|---|---|
| Kill | One target, everyone |
| Slow | One target, everyone |
| Freeze | One target, everyone |
| Fire | One target, everyone |
| Fling | One target, everyone |
| Explode | One target, everyone |
| Kick | One target, everyone |

Paste each id into `OneProductId` and `AllProductId`:

```lua
Kill = {
    Enabled = true,
    OneProductId = 0,
    AllProductId = 0,
    ...
},
```

Set the prices on the website. Nothing in the config carries a price: the buttons read it
live from Roblox, so changing a price on the website is the whole job.

<Callout type="tip" title="Start with fewer">

A troll with `Enabled = false`, or with a product id left at `0`, has its button hidden
entirely. An unbuyable button is worse than no button.

Ship three trolls that work rather than seven that half do.

</Callout>

## 3. Protect your staff

`Configuration/Server/ImmunitySettings.luau`:

```lua
UserIds = { 1234567 },
AttributeName = "TrollImmune",
```

Immune players do not appear in the target list at all, so nobody spends Robux discovering
that somebody is protected.

The attribute is the live version:

```lua
player:SetAttribute("TrollImmune", true)
```

Use it for a player in a cutscene, a staff member on duty, or anybody your own code decides
should be left alone.

<Callout type="warning" title="Immunity is on the server side on purpose">

This file lives in `Configuration/Server`, which is never replicated. An immunity list is an
admin list, and clients have no business reading one.

Do not move it into `Configuration/Shared`.

</Callout>

## 4. Decide about ProcessReceipt

```lua
ManageProcessReceipt = true,
```

If this is the only thing in your game that sells developer products, leave it on.

If you sell anything else, set it to `false` and call this from your own handler:

```lua
local decision = PurchaseService:HandleReceipt(receiptInfo)
```

It returns `NotProcessedYet` for products that are not this system's, and you carry on with
your own handling. See [Products and credits](products.md).

## 5. Press Play with two players

Use the Studio two-player local server. Open the panel, and you should see the other player
in the list with your camera able to follow them.

The buttons on the right hit whoever you are watching; the ones on the left hit everybody.

## 6. Buy one

In Studio, a purchase prompt can be completed without spending real Robux. Buy a Freeze and
watch the other player stop moving for eight seconds.

If the button says nothing and does nothing, the product id is `0` or the product belongs to
another game.

## 7. Tune the damage

Fire and Explode ship lethal against a hundred health:

| Troll | Ships as | Result |
|---|---|---|
| Fire | 15 damage a second for 8 seconds | 120 damage total |
| Explode | 100 blast damage | An instant kill |

Lower `Amount` on both if your game wants survivors. See [The seven trolls](trolls.md).

## 8. Set the cooldown

```lua
AllCooldown = 10,
```

Seconds between two server-wide trolls, counted across the whole server. It is the first
setting to raise when players complain about chaos.

## 9. Turn Debug off

`Configuration/Shared/Settings.luau` ships with `Debug = true`. Set it to `false` before
release. Warnings and errors are always shown either way.

## 10. Publish

Purchases only complete properly in a published game. Everything else, including the
immunity list and the cooldown, behaves the same in Studio.
