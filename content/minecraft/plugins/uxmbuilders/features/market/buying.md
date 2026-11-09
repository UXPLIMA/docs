---
title: Buying a design
order: 3142
description: Browsing, previewing and buying a design, building it from its blueprint, and the design library.
icon: shopping-cart
---

`/builders market` opens the designs on sale. Buying one hands out a blueprint that builds like a
package's: right-click to preview, adjust, confirm, and your builder crew puts it up. What you bought
stays in **My designs**, so a lost blueprint can be issued again.

## Browsing

| Command | Opens |
|---|---|
| `/builders market`, `market browse [category]` | The market, on one category or all |
| `/builders market search <words>` | The market, showing listings whose title, description or seller holds the words |
| `/builders market view <code>` | One listing |

The tabs along the top pick a category. **Sort** cycles newest, most sold, cheapest and dearest first.
**Search** asks for words, in a dialog where the client supports one or in chat, and ignores letter case
and accents; right-click it to clear the search. On a listing:

| Click | Does |
|---|---|
| Click | Opens the listing |
| Right-click | Previews it where you look |
| Shift + click | Lists its materials |

Only listings for sale are shown. Categories whose `permission` you lack, and sellers banned from the
server, are left out.

The listing's own menu shows its details and the builds one purchase allows, a **Preview** and a
**Materials** button, the seller's head (click for more of their designs) and the **Buy** button with
your balance before and after. The button is grey, with the reason, when the listing is yours, sold out,
off sale, when you hold as many copies as you may, or when you cannot afford it. **Report**, shown on
other players' listings, tells the moderators what is wrong with it; see
[Reports](moderation.md#reports).

Bedrock players joining through Geyser or Floodgate get the market, the listing, the confirmation and
My designs as lists of buttons, with `bedrock.menus` on in `config.yml`. A button that does several
things, such as a design in My designs, asks which one. See
[Bedrock forms](../../config/messages-and-menus.md#bedrock-forms).

## Previewing

`/builders market preview <code>`, a right-click in the market or the **Preview** button draws the
design where you look, the same way a [blueprint's preview](../preview.md) does. With an empty hand,
left-click turns it; right-click, which would place a design you own, says to buy it first, with a
**[Buy]** link. `/builders preview` closes it.

## Buying

`/builders market buy <code>` and the **Buy** button open a confirmation with the price and your
balance; **Pay** unlocks after `gui.confirmDelayTicks`. The price charged is the one on that screen:
if the seller changed it meanwhile, the purchase is refused and you review it again.

| Refused when | Setting |
|---|---|
| It is your own listing, sold out, or not for sale | |
| You lack `uxmbuilders.market.buy` or the category's `permission` | |
| Your last purchase was too recent | `cooldowns.purchaseSeconds` |
| You already hold as many copies of this listing as allowed | `purchase.maxPerBuyerLimited`, `maxPerBuyerUnlimited`; `0` is no limit |
| The design is larger than your `uxmbuilders.size.<n>` lets you build | |
| You cannot afford it, or there is no economy plugin | |

The seller is told of the sale at once, or when they next join. When two players on two servers buy
the last copy at the same time, the database lets one of them have it; the other is refunded and told it
sold out.

## The blueprint

The blueprint goes into your inventory, or, when it is full, waits in **My designs**.

| | |
|---|---|
| Builds | `purchase.buildsPerPurchase` per purchase, `-1` for any number. They are counted in the database, so a copied item builds nothing extra |
| Using it | Right-click the ground to preview, left-click to rotate, shift + left-click to mirror, right-click again to start. It stays in hand while builds are left |
| Cost | Nothing: you paid when you bought it |
| Who | Only you, with `purchase.blueprintBound: true`; with `false`, whoever holds the newest copy of the item, using your builds |

Everything else a build checks still applies: [placement rules](../placement-rules.md), land protection
where you build, your build limit, your builder, and the [warehouse](../warehouse.md) when materials are
on. Your build history and the job menu show the design's title.

## My designs

`/builders market designs` (or `library`) lists what you bought, newest first, with its status, builds
left and price. It works while the market is closed.

| Click | Does |
|---|---|
| Click | A new blueprint. Every older copy of it stops working. Needs a free inventory slot, and waits `cooldowns.reissueSeconds` between two for the same design |
| Right-click | Previews it where you look |
| Shift + click | Lists its materials |

A design shows **Revoked** or **Refunded** when a moderator took the licence back; it starts no new
build, while a build already running carries on.

## Cancelling a market build

No money moves when a market build is cancelled, and the seller keeps what they earned. What can come
back is the build itself, the right to build the design once more:

| Cancelled | Build given back |
|---|---|
| By an admin, or by a server error | Always |
| By you, or for any other reason | When no more than `purchase.creditReturnMaxProgress` percent (10) was built |
| By a restart | Not cancelled: the build resumes |

`/builders cancel` asks for `/builders confirm` with the share up to which the build comes back, rather
than a refund (`infoCancelConfirmRequiredMarket`). The notice after it says whether the build came back,
to be placed again from **My designs** (`infoBuildCancelledCreditBack`), or is used up
(`infoBuildCancelledCreditUsed`). The blocks already placed stay, as with any cancelled build.

## Notes

- `uxmbuilders.market.bypass.cooldown` skips the purchase and re-issue waits; `bypass.limit` skips the
  copies-per-buyer limit.
- A refund by a moderator never puts a limited copy back on sale.

Related: [Selling a build](selling.md), [Player market](index.md), [Preview](../preview.md)
