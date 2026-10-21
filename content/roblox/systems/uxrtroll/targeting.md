---
title: Targeting and immunity
order: 3
description: The spectate list, self-targeting, who is spared, and the server cooldown.
icon: crosshair
---

## Two scopes

| Scope | Hits |
|---|---|
| `One` | The player you are currently watching |
| `All` | Everybody in the server |

The panel puts them on two sides: the right-hand buttons are single target, the left-hand
buttons hit everyone.

## The target list

The panel lists the players you may target, sorted by display name, and the camera follows
whichever you are on. Stepping through the list moves the camera with it.

The list updates when anybody joins or leaves, so a target who has left cannot be selected.

<Callout type="info" title="Immune players are absent, not greyed out">

An immune player never appears in the list. That is deliberate: a greyed-out entry tells
everybody who is protected, and an absent one tells them nothing.

</Callout>

## Immunity

`Configuration/Server/ImmunitySettings.luau`:

```lua
UserIds = {},
AttributeName = "TrollImmune",
```

Two ways to be immune:

| | |
|---|---|
| A user id in `UserIds` | Permanent |
| The attribute, set to `true` | Live, for as long as your code wants |

```lua
player:SetAttribute("TrollImmune", true)
```

Set `AttributeName = ""` to switch the attribute check off entirely and use only the list.

Immunity is checked in three places: when the list is built, when a single-target troll is
delivered, and when an everyone troll picks its victims. There is no path that reaches an
immune player.

## Self-targeting

```lua
AllowSelfTarget = true,
```

Whether a player may put themselves in the target list and buy a single-target troll on
themselves.

Harmless, and some players enjoy it. `false` removes them from their own list.

## Does an everyone troll hit the buyer

```lua
AllIncludesBuyer = false,
```

Off by default. Paying to kill yourself reads as a bug rather than as a joke.

Turn it on if your game's tone is that a server-wide troll is genuinely indiscriminate.

## The cooldown

```lua
AllCooldown = 10,
```

Seconds between two everyone trolls, counted **across the whole server** rather than per
player.

A player who presses too soon is told privately how long is left, and no purchase prompt
opens.

<Callout type="tip" title="This is your main chaos dial">

Ten seconds is playable. On a busy server with a lot of buyers it is still a troll every ten
seconds, which for some games is the point and for others is unbearable.

Raise it to 30 or 60 before you consider disabling the everyone buttons.

</Callout>

Single-target trolls have no cooldown of their own. The rate limit in
`Configuration/Shared/Settings.luau` is a general anti-flood guard, not a game rule:

```lua
Security = { MaxRequestsPerSecond = 10 },
```

## What happens when a troll is refused

| Situation | Result |
|---|---|
| The troll is disabled | Nothing happens. The button is hidden anyway |
| The cooldown is running | A private chat line, no prompt |
| No target selected for a single troll | The purchase becomes a credit |
| The target is immune | The purchase becomes a credit |
| The target left | The purchase becomes a credit, with a private line |

A purchase is never simply lost. See [Products and credits](products.md).

## Everyone trolls with nobody to hit

An everyone troll that reaches nobody, because the server is empty or everybody in it is
immune, is not marked as granted. Roblox keeps the receipt and retries it later, so the
purchase is not lost: it lands the next time it is retried and there is somebody to hit.

That is an edge case worth knowing about on a small server with a large staff.
