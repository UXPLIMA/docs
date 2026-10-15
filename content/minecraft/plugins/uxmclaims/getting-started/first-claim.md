---
title: Your first claim
order: 104
description: "Claim a chunk, invite a friend, set the rules: start to finish."
icon: play
---

Every step below also works from the menus. The commands are shown because they are shorter to write
down. With LuckPerms, each step also needs its [ability node](../permissions/abilities.md).

## 1. Claim the chunk you are standing on

```
/claim
```

With `generalSettings.smartCommand: true` (the default) a bare `/claim` looks at where you stand:

| Where you stand | What happens |
|---|---|
| In a claim you belong to | Opens that claim's menu |
| In a claim you do not belong to | Opens the main menu |
| On free land sharing a side with a claim you own | Buys the chunk and adds it to that claim |
| On other free land | Asks you to confirm, then creates a claim |

With `smartCommand: false`, the last two rows open the main menu instead. `/claim create MyBase` creates a
claim without the confirmation and names it. The claim block appears where you stand.

The new claim becomes your selected claim. Every command below acts on the selected claim, or on the
claim you stand in when nothing is selected. `/claim select <claim>` picks another one.

## 2. See what you own

```
/claim showborders
```

This toggles the outline of the claim you stand in: green when you belong to it, red when you do not,
blue for an unclaimed chunk. It closes after `border.autoCloseSeconds`, 30 by default. `/claim chunk view`
opens the claim map instead.

## 3. Grow it

Walk to a free chunk that shares a side with your claim and run:

```
/claim chunk add
```

The chunk costs `uxmclaims.cost.chunk`, `25.0` by default, and a claim holds at most
`uxmclaims.limit.chunk` chunks, 10 by default. `/claim chunk remove` gives up the chunk you stand in after
a confirmation. It refunds nothing.

## 4. Invite someone

```
/claim trust Steve
```

Steve must have joined the server before. He gets a message with **[Accept]** and **[Reject]**, and
`/claim invites` opens his inbox if he missed it. Until he accepts he is a visitor. Once he accepts he holds
the `Member` role, which may build, open containers, use redstone and doors, ride and handle animals, but
manage nothing.

## 5. Set the house rules

```
/claim setflag PVP false
/claim toggleflag TNT_EXPLOSIONS
```

Or open `/claim` and click *Flags*, which lists all 32 flags over two pages. Only the owner may change
flags. [Flags](../protection/flags.md) says what each one does.

## 6. Make a spawn and a warp

```
/claim setspawn
/claim warp create shop
/claim warp setpublic shop true
```

A public warp appears in `/claim warps` for everyone on the server. When the claim has `PVP` on, a player
teleporting to one of its public warps is asked to confirm first. A claim holds 3 warps by default
(`uxmclaims.limit.warp`).

## 7. Give someone a custom role

```
/claim role create Builder
/claim role setperm Builder MOVE_INSIDE true
/claim role setperm Builder BLOCK_PLACE true
/claim role setperm Builder BLOCK_BREAK true
/claim member setrole Steve Builder
```

A new role starts with no permissions, which is why `MOVE_INSIDE` comes first: without it Steve would be
pushed out of the claim. Then take one thing back from Steve alone:

```
/claim member setperm Steve BLOCK_BREAK false
```

The per-member deny wins over the role. Steve can place blocks but no longer break them.

## 8. Keep it alive

With `claimSettings.expireMode: true` (the default), a claim is deleted when its time runs out:
`claimSettings.expireTime`, 30 days, after creation. Extend it with the *Extend Time* menu or:

```
/claim extend 7d
```

The duration accepts `d`, `h`, `m` and `s`, combined as in `1d12h30m`. The price is
`uxmclaims.cost.time` per second per chunk: the default `5.0` makes one day on a one-chunk claim cost
432,000, so set it to suit your economy. The time left after extending may not exceed
`uxmclaims.limit.time`, 30 days by default, so a brand new claim cannot be extended until some of its
time has passed. Owners get a reminder once less than `generalSettings.reminderThresholdSeconds` (one
day) is left.

<Callout type="warning" title="Turning expiry back on deletes overdue claims">

With `expireMode: false` claims are never deleted and `/claim extend` is refused, but every claim's
expiry date keeps passing. Set `expireMode` back to `true` and every claim whose date has passed is
deleted within seconds. Raising `claimSettings.expireTime` does not save them: it only applies to new claims.

</Callout>
