---
title: Selling a build
order: 3141
description: Selecting a build, the capture and what it leaves out, the listing, managing it, and being paid.
icon: tag
---

Selling takes three steps: mark the build's two corners, run `/builders market sell`, and fill in the
listing. The plugin reads the build into a design, cleans it, and opens the listing form; publishing
pays the listing fee and puts the design on sale.

## Selecting the build

| Command | Does |
|---|---|
| `/builders market wand` | Gives the selection wand: left-click a block for the first corner, right-click for the second |
| `/builders market pos1`, `pos2` | Marks the block you look at, up to 10 blocks away, as the first or second corner |
| `/builders market clear` | Forgets the corners |

Each corner is confirmed in chat, then the selection's size and volume. Both corners must be in the
same world. Without wand corners, the WorldEdit selection is used when WorldEdit is installed. The wand
is the same item as `/builders admin wand` and marks corners for holders of `uxmbuilders.market.wand`.

## The capture

`/builders market sell`, or **Sell a build** in the market menu, checks the seller and the place before
anything is read:

| Check | Refused when |
|---|---|
| Size | Wider, longer or taller than `capture.maxWidth`, `maxLength`, `maxHeight`, or more volume than `capture.maxVolume` or the seller's `uxmbuilders.market.size.<n>` |
| World | `capture.worlds` does not allow it |
| Seller | No `uxmbuilders.market.sell`, or banned from the market |
| Cooldown | `cooldowns.createSeconds` since the seller's last listing has not passed |
| Live listings | The seller already has as many as `uxmbuilders.market.listings.<n>`, or `listing.maxLiveDefault` |
| Land | `capture.requireBuildPermission` is on and a protection plugin says the seller may not build on part of the box, or the box is on nobody's land and `capture.wilderness` is `DENY` |

The world is then read without generating land: a chunk that was never generated fails the capture, and
reading stops after `capture.timeoutSeconds`. A player runs one capture at a time, and
the server runs two at once; others wait their turn.

<Callout type="warning" title="Without a protection plugin, anything can be sold">

The land check asks your protection plugins. With none installed, or `protection.mode` set to `NONE`,
there is nothing to ask: any build can be captured, other players' builds included, and the console says
so once. `uxmbuilders.market.bypass.protection`, held by operators, skips the check.

</Callout>

## What the capture leaves out

Buyers build a design block for block, and with the warehouse off every block is free. The capture
therefore removes what no player should receive that way:

| What | Becomes |
|---|---|
| Portals, TNT, fire, spawners, trial spawners, vaults, budding amethyst, reinforced deepslate, the dragon egg, wither skeleton skulls, bedrock, barriers, light, structure and jigsaw blocks, command and test blocks | Air, or the capture is refused with `capture.blacklistPolicy: REJECT` |
| `schematic.blacklist` in `config.yml` and `capture.extraBlacklist` | The same |
| `capture.valuableBlocks` (diamond, emerald, netherite, gold and iron blocks, ancient debris, beacons, conduits, shulker boxes) | Air while `materials.enabled` is off, since every built block is then free |
| Water, lava and bubble columns | Air, and nothing stays waterlogged, unless `capture.allowFluids` is on |
| A lectern's book, a jukebox's record, filled bookshelf slots and brewing stand bottles, a composter's or beehive's level, an anchor's charges, a shrieker's summoning, unstable TNT | Reset to empty |
| Lava, water and powder snow cauldrons | An empty cauldron |
| What chests, barrels and other containers hold | Never read: the block is kept, its contents are not |

Sign text passes the word filter (`text.blockedWords`, `blockedPatterns`); a blocked word refuses the
capture and names the sign's position. Player heads keep their skin. With `capture.trimAir` the box
shrinks to the blocks it holds, and a design with fewer than `capture.minBlocks` blocks is refused. The
chat reports the size, the block count and what was left out.

The capture is kept for ten minutes while you fill in the listing.

## Duplicates

The design is compared in all four rotations and mirrored:

| Refused when | Setting |
|---|---|
| It is already on sale | `listing.dedupe`: `GLOBAL`, once on the whole network; `SELLER`, once per seller, so co-owners may each list it |
| You bought it on the market | `listing.blockResaleOfPurchased` |
| You sold it before as a limited edition and at least one copy sold | `listing.lockLimitedEditions` |

## The listing form

On Paper, for a client on 1.21.7 or later and with `dialog.enabled` in `config.yml`, the form is one
dialog screen. Otherwise chat asks one question at a time; type `cancel` to stop.

| Field | Rules |
|---|---|
| Title | `listing.titleMin` to `titleMax` characters; `<` and `>` refused, colour codes and invisible characters removed, the word filter applies |
| Description | Up to `listing.descriptionMax`; in chat, `-` leaves it empty. With `descriptionMax: 0` it is not asked |
| Price | `economy.priceMin` to `priceMax`; `1500`, `1,500`, `1.5k` and `2m` are read |
| Copies | 1 to `listing.limitedMaxCopies`, or unlimited when `listing.allowUnlimited`; in chat, `0` is unlimited |
| Category | One of the categories in `market.yml` whose `permission` you hold |

The confirm menu then shows the listing as it will be, the listing fee, your balance, the commission and
the blocks left out. Pick the icon from the design's most used blocks or the item in your hand (its type
only), cycle the category, or go back to the form. **Publish** unlocks after `gui.confirmDelayTicks`
and is grey while the fee is more than your balance. Publishing charges the fee and lists the design at
once, with its code.

## Fee and commission

| | Default | Per player |
|---|---|---|
| Listing fee | `economy.listingFee` | `uxmbuilders.market.fee.exempt` pays nothing |
| Commission | `economy.commissionPercent` of each sale | `uxmbuilders.market.commission.<n>` percent, the lowest held |

The commission is set when a copy sells. A seller's `commission.<n>` node is read only while the seller
is online on the server where the copy sells; otherwise `economy.commissionPercent` applies.

## Managing listings

`/builders market mine` lists your listings with their status, and live listings against your limit.
Paused, sold out and under-review listings count as live: withdraw a sold-out listing to free its place.

| Button | Does |
|---|---|
| Change the price | Once every `listing.priceChangeCooldownMinutes`, counted from the listing or the last change. A buyer who saw the old price is refused at checkout, not charged the new one |
| Title, description | The same rules as the form |
| Category, icon from your hand | Cycles the category; takes the held item's type |
| Pause, Resume | Takes it off sale for now, and back. A listing under review cannot be resumed |
| Withdraw (shift-click) | Ends the listing for good. Buyers keep what they bought; the fee is not returned |

## Earnings and payouts

Each sale's net is owed to the seller in the market's books, then paid into their balance:

| `economy.payout` | Paid |
|---|---|
| `INSTANT` | Right after the sale when the seller is online on that server, within a minute on another server, otherwise when they join |
| `ON_JOIN` | When they join, or ask with `/builders market claim` or the **Earnings** button of their listings |

`/builders market earnings` shows the lifetime total, what was paid, what is still to be paid, and what
is on hold under a [market ban](moderation.md#bans). A seller who was away is told two seconds after
joining how many copies sold and what they earned. A payout the economy refuses stays owed and is tried
again; a seller banned from the server is paid once the ban ends. When a moderator refunds a buyer, the
net is taken back from the seller: if it was already paid, the seller's next earnings pay it off first.

## Notes

- `uxmbuilders.market.bypass.cooldown` skips the listing wait, `bypass.limit` the live listing cap and
  `market.size.<n>`, `bypass.filter` the word filter. `capture.maxVolume` and the other size limits hold
  for everyone.
- Abandoned captures leave nothing behind: nothing is stored until a listing is published.
- A listing's seller name follows the player's current name, updated when they join.

Related: [Buying a design](buying.md), [Market moderation](moderation.md), [market.yml](../../config/market-yml.md)
