---
title: Welcome
order: 101
description: Crafting that takes real time, can fail, and levels the player up.
icon: door-open
---

uxmBlacksmith replaces "put items in a grid, get an item" with a contract. The player hands materials
to the blacksmith, the work takes **real time**, it can **fail**, and finishing it earns **experience**
in a progression system with levels, per-category masteries and a perk tree.

Requires **Minecraft 1.16 or newer**.

## The four ideas

| Idea | What it means |
|---|---|
| **Time gating** | A trade has a duration. Hand over the materials, come back in three hours. |
| **Burn risk** | A trade has a success chance. Below 100% it can fail and consume the materials. |
| **Slots** | A player runs a limited number of trades at once, and slots are a permission. |
| **Progression** | Every completed trade grants XP, mastery in its category, and perk points. |

## What that buys you

The reason to time-gate a recipe is that the strongest item on the server stops being a function of
how many diamonds someone has. It becomes a function of how many slots they hold, how long they are
willing to wait, and whether they get lucky.

The burn system is the gamble. An 85% success chance means one attempt in seven destroys the
materials, and rank permissions can raise that chance, which is a straightforward thing to sell.

## Everything in the box

- Trades with a duration, a success chance, a reward item or a command, and arbitrary requirements
- Requirements from items, money, PlayerPoints, XP, CoinsEngine, or a **PlaceholderAPI condition**,
  so "VIP only" and "1 hour playtime" are requirements like any other
- Custom-item hooks for MMOItems, EcoItems, ExecutableItems, ItemsAdder and Nexo
- A binary item store: save any item in-game and use it as a requirement or reward
- Boost items that speed up trades or finish one instantly
- A global blacksmith level, per-category masteries, and a configurable perk tree
- Vanilla crafting gates: require a blacksmith level to craft a normal recipe
- An admin analytics layer with daily statistics, per-player history and CSV/JSON export
- SQLite storage with HikariCP and WAL

<Callout type="info" title="The repository is named ancBlacksmith">

The project's git repository and some legacy permission nodes still use the `anc` prefix. The plugin,
its command and its current permission nodes are `uxmblacksmith`. Where a legacy name still works, this
documentation says so.

</Callout>

## Where to go next

| You want | Page |
|---|---|
| Install it | [Installation](installation.md) |
| Learn the vocabulary | [Concepts](concepts.md) |
| Build a trade end to end | [Your first trade](first-trade.md) |
| Understand burn chance | [Burn and risk](../features/burn.md) |
| Design the perk tree | [Perks](../features/perks.md) |
