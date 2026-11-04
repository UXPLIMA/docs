---
title: Your First Region
order: 1050
description: Protect a spawn in five commands, then check it the way a player would meet it.
icon: footprints
---

This walk-through protects a spawn: nobody may build, fight or light fires there, doors and buttons still
work, and players get a greeting on the way in. You need operator rights, or the `uxmessentials.guard.*` nodes.

## 1. Mark the area

Take the wand and click two opposite corners of the spawn:

```text
/guard wand
```

Left click a block for the first corner, right click for the second. With WorldEdit or FAWE installed you can
use their wand instead; guard reads that selection.

## 2. Define the region

```text
/guard expand vert
/guard define spawn
```

`expand vert` stretches your selection from the bottom of the world to the build limit, so the region covers
every height above and below the spawn. For a round spawn, stand in the middle and run
`/guard define spawn cylinder 40` instead.

The region exists now and protects nothing yet: a new region has no rules.

## 3. Link the spawn template

```text
/guard template spawn spawn
```

The `spawn` template (in `modules/guard/config.conf`) refuses building, using blocks, fighting, dropping items,
ender pearls, buckets and lighters, trampling, explosions, fire and mob grief, and lets doors, gates, buttons
and pressure plates work. It makes players inside invincible and stops monsters from spawning. The
[Templates](templates.md) page lists every template line by line.

## 4. Add your own touches

Rules and behaviours you set on the region win over the template:

```text
/guard set spawn message.enter <gold>Welcome to the server, <white>{player}
/guard set spawn message.channel title
/guard rule spawn use.container role:member allow
/guard role spawn Notch member
```

Now Notch may open chests at spawn, and everyone else may not.

## 5. Check it

```text
/guard info spawn
/guard show spawn
/guard test Steve break
```

`info` prints the shape, rules, roles and behaviours. `show` draws the border for you alone. `test` answers
whether Steve may break a block where you stand (or, with a region name after the action, in the middle of that
region), and why.

A player who tries to break a block in spawn now sees a short refusal on the action bar. If one of them asks
why, `/guard why break` (open to everyone) explains it to them where they stand.

## What next

- Prefer clicking: `/guard menu` opens the same region in a window. See [Menus](menus.md).
- Moving from WorldGuard: [Importing WorldGuard](worldguard.md) does this for every region at once.
- Several lobbies sharing one spawn: [Networks](networks.md).
