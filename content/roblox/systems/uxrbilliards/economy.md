---
title: Rating, rewards and cosmetics
order: 6
description: The ELO ladder, coin payouts, the cue catalogue and the Robux hooks.
icon: coins
---

## Rating

```lua
Config.elo = { start = 1000, K = 32 }
```

Standard ELO. `K = 32` is the classic value: an evenly matched win moves a player by 16.

Only human matches are rated. Solo and AI games are excluded entirely, so a player cannot
farm a rating against a difficulty they chose.

## leaderstats

```lua
Config.leaderstats = { enabled = true, elo = "Elo", wins = "Wins" }
```

Two columns in the player list. Set either to `false` to hide it, or `enabled = false` for
neither.

## Leaderboards

```lua
Config.leaderboard = {
    stats = { "Wins", "Losses", "Draws", "Elo" },
    topN = 25,
    refreshSeconds = 60,
    storePrefix = "uxrBL_LB_",
    storeVersion = "v1",
    workspaceFolder = "uxrBilliardsWorkspace",
    boardsFolder = "Leaderboards",
},
```

In-world boards, built by you in `Workspace/uxrBilliardsWorkspace/Leaderboards`, one model
per entry in `stats`.

| Model name | Shows |
|---|---|
| `<Stat>Leaderboard` | The global top players across every server |
| `<Stat>ServerLeaderboard` | Only the players in this server |

Each board needs a `ListPart.ListGui.ScrollingFrame` containing a `Template` row with
`RankTextLabel`, `NameTextLabel` and `ValueTextLabel`.

Bumping `storeVersion` starts a fresh season without touching profiles.

## Coins

```lua
Config.rewards = { winCoins = 50, playCoins = 5 }
```

| Field | Default | Paid |
|---|---|---|
| `winCoins` | `50` | To the winner |
| `playCoins` | `5` | For finishing a match at all |

Unlike most systems in the range these are non-zero out of the box, because coins have
somewhere to go: the cue shop.

## The cue catalogue

`Shared/Billiards/Cosmetics.luau` holds the cues. Each entry is:

```lua
{
    id = "cue_katana",
    category = "cue",
    name = "Katana",
    cost = { coins = 0 },
    apply = { kind = "mesh", asset = "katana" },
},
```

| Field | What it is |
|---|---|
| `id` | The internal id, stored in the profile |
| `category` | `"cue"` today |
| `name` | What the shop shows |
| `cost` | `{ coins = n }`. Zero is free |
| `default` | Marks the cue everyone starts with |
| `apply` | How it is rendered: `{ kind = "none" }` or `{ kind = "mesh", asset = "…" }` |

Five ship: a classic cue and four novelty ones. All are priced at zero coins as shipped, so
the shop is a wardrobe until you set costs.

Adding a cue means adding an entry and providing the mesh named by `asset` in your assets
folder. Ownership is stored per player in the profile.

<Callout type="warning" title="The shipped cues are all free, which makes the coin economy decorative">

`winCoins` and `playCoins` pay out, and there is nothing to spend them on until you price
the catalogue. Set real costs on the novelty cues before launch, or players accumulate a
balance with no purpose.

</Callout>

## Robux

```lua
Config.coinProducts = {}
Config.vipGamepass = 0
```

`coinProducts` maps a Developer Product id to a number of coins:

```lua
Config.coinProducts = {
    [1234567890] = 1000,
    [1234567891] = 5000,
}
```

`vipGamepass` is a Gamepass id that unlocks every cosmetic. `0` disables it.

Both ship empty, which means no Robux flow is live until you create the products and paste
their ids in.

<Callout type="danger" title="One system must own ProcessReceipt">

`MarketplaceService.ProcessReceipt` is a single global callback and the last script to set
it wins. If anything else in your game sells Developer Products, decide which system owns
the callback and route the others through it.

Get it wrong and one system's purchases are charged and never delivered, silently. See
[Installing a system](../platform/installation.md).

</Callout>
