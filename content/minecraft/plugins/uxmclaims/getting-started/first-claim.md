---
title: Your first claim
order: 104
description: "Claim a chunk, invite a friend, set the rules: start to finish."
icon: play
---

Everything below is also in the menus. Commands are shown because they are shorter to write down.

## 1. Claim the chunk you are standing on

```
/claim
```

With `generalSettings.smartCommand: true` (the default) a bare `/claim` is context aware:

| Where you are | What happens |
|---|---|
| In your own claim | Opens the claim's management menu |
| In someone else's claim | Opens your personal menu |
| On free land next to your own claim | Buys that chunk and adds it to the neighbouring claim |
| On free land elsewhere | Asks you to confirm, then creates a new claim |

Set `smartCommand: false` and a bare `/claim` always opens the menu instead.

`/claim create MyBase` skips the guessing and names the claim as it creates it.

## 2. See what you own

```
/claim showborders
```

Green is yours, red is someone else's, blue is wilderness. The overlay closes itself after
`border.autoCloseSeconds` seconds, 30 by default.

## 3. Grow it

Walk to a chunk touching your claim and run:

```
/claim chunk add
```

The chunk costs whatever `uxmclaims.cost.chunk` resolves to, `25.0` by default, and counts against
`uxmclaims.limit.chunk`. `/claim chunk remove` sells one back.

## 4. Invite someone

```
/claim trust Steve
```

Steve gets a clickable accept/reject message. Until he accepts he is not a member and holds no
permissions. `/claim invites` opens his inbox if he lost the message.

Once he accepts he holds the `Member` role, which by default may build, open containers, use redstone,
ride and interact, but not manage anything.

## 5. Set the house rules

```
/claim setflag PVP false
/claim toggleflag TNT_EXPLOSIONS
```

Or open the flags menu from `/claim` → *Claim Flags*, which lists all 32 with their current state.

## 6. Make a spawn and a warp

```
/claim setspawn
/claim warp create shop
/claim warp setpublic shop true
```

A public warp appears in `/claim warps` for everyone on the server. If the claim has `PVP` on, players
teleporting to a public warp get a confirmation prompt first: the plugin will not drop a stranger
into a PvP zone silently.

## 7. Give someone a custom role

```
/claim role create Builder
/claim role setperm Builder BLOCK_PLACE true
/claim role setperm Builder BLOCK_BREAK true
/claim member setrole Steve Builder
```

Then take one thing back, just from Steve:

```
/claim member setperm Steve CONTAINER_OPEN false
```

The per-member deny wins over the role.

## 8. Keep it alive

If `claimSettings.expireMode` is on, a claim expires after `expireTime` (30 days by default) and is
deleted. Extend it:

```
/claim extend 7d
```

The duration parser accepts `d`, `h`, `m` and `s` and combinations like `1d12h30m`. The cost is
`uxmclaims.cost.time` per second, `5.0` by default, so price your ranks accordingly.

Players get a warning when a claim is within `reminderThresholdSeconds` (one day) of expiring.

<Callout type="tip" title="Turn expiry off for a survival server">

`expireMode: true` is the right default for a busy server that must reclaim abandoned land. On a small
whitelisted server it mostly generates support tickets. Set `claimSettings.expireMode: false` and the
whole timer, the reminders and the extend cost disappear.

</Callout>
