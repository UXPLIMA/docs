---
title: The personal menu
order: 402
description: The hub a player sees outside their own land.
icon: home
---

Opened by `/claim menu`, or by `/claim` anywhere you are not a member.

Layout: `menu/common_main.yml`.

| Button | Opens |
|---|---|
| Your claims | The list of claims you own or belong to |
| Public warps | Every published warp on the server |
| Invitations | Your pending invitation inbox |
| Claim this chunk | Creates a claim where you stand, if the chunk is free |

## Your claims

`menu/common_list.yml`. Paginated, filling the slots listed in `itemSlots`.

Each entry shows the claim's name, its owner and its remaining time. Click to manage it, shift-click
to select it — selection is what makes `/claim role create …` work from anywhere.

The currently selected claim renders through the `…Selected` item variant, with a glint and a tick in
the name.

## Public warps

`menu/common_warp_list.yml`. Every warp with `isPublic` set, from every claim.

Clicking teleports, after the `uxmclaims.delay.teleport` warmup and any
`uxmclaims.cost.warptp.public` charge. If the owning claim has the `PVP` flag on, a confirmation
screen appears first.

## Invitation inbox

`menu/common_invite_inbox.yml`. Everything waiting for an answer, with accept and reject on each
entry. The same list `/claim invites` opens.

## The picker

`menu/common_selection.yml`, opened by a bare `/claim select`. Choosing a claim here is identical to
`/claim select <name>` and persists until you select something else or the claim goes away.
