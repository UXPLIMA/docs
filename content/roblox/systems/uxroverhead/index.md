---
title: uxrOverhead
order: 40
description: Nametags above every player, driven by ranks, condition-checked icon tags, AFK state and a health bar.
icon: user-round
---

The label above a player's head. Team, display name, rank, a row of icon tags, a health bar
and an AFK state, all assembled from configuration rather than code.

It replaces the default Roblox name and health display.

## Pages

- [Setup](setup.md)
- [The layout](layout.md)
- [Ranks](ranks.md)
- [Icon tags](nametags.md)
- [Conditions](conditions.md)
- [AFK and health](afk.md)
- [Settings](settings.md)

## What it does

| | |
|---|---|
| Layout | An ordered list of template rows with placeholder text |
| Ranks | Condition-matched titles with their own colours, resolved by priority |
| Icon tags | Device, country flag, and any number of condition-driven badges |
| Conditions | Eighteen check types: group, rank, team, gamepass, premium, level and more |
| AFK | Window-focus detection with a timer and optional transparency |
| Health | A bar that follows the humanoid |
| Chat state | An icon shown to viewers who cannot chat with that player |
| Integration | Optional, reads level and rank from uxrLevel if present |

## The idea

Everything a nametag shows is one of three things:

1. a **row**, listed in `Layout.luau` with placeholder text
2. a **rank**, matched in `Ranks.luau` by a condition and carrying colours
3. an **icon tag**, matched in `Nametags.luau` by a condition

All three run through the same condition engine. Adding "a gold VIP badge for anyone with
gamepass 12345" is one entry, not a script.
