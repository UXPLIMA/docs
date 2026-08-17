---
title: Your First Island
order: 490
description: A player's first island, step by step, from /is to a working base.
icon: flag
---

This page walks a player through creating an island and getting started, step by
step. Every command below uses the default name `/is`: your server may have
renamed it (see [config.yml](../configuration/config-yml.md)).

---

## Step 1: Create Your Island

```
/is create
```

This opens the **island type** menu: click the starter island you want and it's
built instantly. If you already know the type, skip the menu by naming it:

```
/is create desert
```

The plugin builds your island and teleports you to it. The first player on the
island is the **owner**.

<Callout type="tip" title="Nether &amp; End">

Your island comes with its own **Nether** and **End** plots. Build a Nether
portal on your island to reach them: there's a ready End portal waiting in your
Nether plot.

</Callout>

<Callout type="info" title="One island per player">

A player owns one island at a time. To start over, delete the current one with
`/is delete` first.

</Callout>

---

## Step 2: Look Around

You're standing on your starter island. A colored **border** marks how far your
protected area reaches. Outside the border, nothing is protected.

Open the main menu any time with:

```
/is
```

From here you reach settings, upgrades, warps, the bank, and your team.

---

## Step 3: Invite Your Friends

```
/is invite Steve
```

Steve runs `/is accept` to join. New members get the default **member** role.
Change anyone's role with:

```
/is role Steve architect
```

See [Roles & Permissions](../protection/roles-permissions.md) for what each role can do.

---

## Step 4: Start Leveling Up

Your island has a **level**, raised by the value of the blocks you place. Mine
cobblestone, smelt ores, build with valuable blocks: every block placed adds
points (breaking it removes them).

Check your progress:

```
/is level
/is top
```

What each block is worth is in [block-values.yml](../configuration/block-values-yml.md),
and how points become levels is in [levels.yml](../configuration/levels-yml.md).

---

## Step 5: Buy Upgrades

As your level rises, you unlock upgrades:

```
/is upgrade
```

Bigger team, bigger island, faster spawners, double crops, and better
**generators** that roll ores instead of plain cobblestone. See
[Upgrades](../progression/upgrades.md).

---

## Step 6: Open Up to Visitors

Set a public warp so people can find your island:

```
/is setwarp shop
```

Anyone can then reach it with `/is warp <your-name> shop` or the warp menu. Lock
your island to visitors any time with `/is lock`.

---

## Quick Reference

| Goal | Command |
|------|---------|
| Make an island | `/is create [type]` |
| Go home | `/is home` |
| Open the menu | `/is` |
| Invite a friend | `/is invite <player>` |
| Buy upgrades | `/is upgrade` |
| Set a warp | `/is setwarp <name>` |
| See rankings | `/is top` |

Next: [🧠 Core Concepts](concepts.md)
