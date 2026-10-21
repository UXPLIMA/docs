---
title: The interface
order: 6
description: The instance names the panel needs, the four card states, and re-skinning it.
icon: layout-panel-top
---

The panel is a `ScreenGui` named `uxrDailyRewardsSystemGui` under `Client/`. You own its
appearance completely: colours, sizes, corner radii, fonts, images, animations, the
layout of the grid. The code only looks for a small set of names and never writes styling
back.

## The instance contract

```
uxrDailyRewardsSystemGui
  RewardFrame
    RewardList
      ScrollingFrame
        UIGridLayout
          RewardTemplate          the prototype, cloned once per day
            PrizeImage
            ValueText
            TimeText
            ClaimButton
    SkipButton
      ButtonLabel
    CloseButton
  Sound                           optional, played on a successful claim
  CoreScript                      the code. Do not rename or move it
```

Anything not in this list is yours: background art, a title, a glow, particles, a UIStroke
on the template. Add what you like.

| Instance | Required | What the code does with it |
|---|---|---|
| `RewardFrame` | Yes | Shown and hidden. Missing means the page does nothing |
| `RewardList.ScrollingFrame` | Yes | Cards are parented here |
| `UIGridLayout.RewardTemplate` | Yes | Cloned once per day |
| `PrizeImage` | No | `Image` set from the day's `Icon` |
| `ValueText` | No | `Text` set from the day's `ValueText` |
| `TimeText` | No | Live countdown, shown only on the pending day |
| `ClaimButton` | No | Made active on the ready day, fires the claim |
| `SkipButton` | No | Prompts the Developer Product |
| `SkipButton.ButtonLabel` | No | `Text` set from `PriceText` |
| `CloseButton` | No | Hides the panel |
| `Sound` | No | Played on a successful claim |

An optional instance that is missing is simply not used. A missing required one puts a
warning in Output and leaves the page idle rather than erroring.

## Why the template lives inside the layout

`RewardTemplate` is parented to the `UIGridLayout`, not to the `ScrollingFrame`.

A `GuiObject` under a `UILayout` is not laid out and does not render, so the template sits
there as an invisible prototype without needing `Visible = false` juggling. Clones are
parented to the `ScrollingFrame`, where they are siblings of the layout and therefore
arranged by it.

It also makes clearing the list trivial: everything directly under the `ScrollingFrame` is
a clone, so the code can delete all of them without special-casing the template.

<Callout type="warning" title="Moving the template out of the UIGridLayout breaks the grid">

Parent it to the `ScrollingFrame` and it becomes a real card: it renders, it takes a grid
cell, and the first refresh deletes it, after which no cards can be built at all.

</Callout>

## The four card states

| State | When | What changes |
|---|---|---|
| `claimed` | Day is before the current day | Card is faded, countdown hidden, not clickable |
| `ready` | The current day and claimable now | Full opacity, clickable |
| `pending` | The current day, still on cooldown | Full opacity, countdown visible, not clickable |
| `locked` | A day after the current one | Faded, countdown hidden, not clickable |

Fading uses `ImageTransparency` on the card. Everything else about how a state looks is
your art, which is the point: a state is communicated by what you drew, not by the code
recolouring your design.

## ClaimButton is an invisible click layer

The designer's `ClaimButton` is meant to be a full-card, transparent button:
`Text = ""`, `BackgroundTransparency = 1`, `AutoButtonColor` off. The code toggles only
whether it accepts input.

Do not put a label or a background on it. The card art shows the state; the button just
catches the click.

## Opening it from your own button

```lua
-- in a LocalScript, once the panel exists
local ctx = -- the interface context
ctx.DailyRewards.open()
ctx.DailyRewards.close()
```

`AutoOpenOnJoin` handles the common case, and only opens when a reward is actually ready.
Use `open()` for a rewards button on your own HUD.

## The countdown

The pending card's `TimeText` ticks down once a second on the client. When it reaches
zero the page pulls fresh state from the server rather than deciding for itself that the
reward is ready.

That matters: the client's countdown is a display, not an authority. A player who edits
it locally gains nothing, because the claim is checked again on the server.

## Re-skinning it

Work on the copy under `ServerScriptService/uxrDailyRewardsSystem/Client/`, not the one in
`StarterGui`. The bootstrap clones the package copy into `StarterGui` at startup, and only
when no `ScreenGui` of that name is already there, so edits made to the `StarterGui` copy
last until the next Play.
