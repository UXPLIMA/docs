---
title: Bedrock (Floodgate)
order: 1530
description: What Bedrock players get, and what Floodgate has to be installed for.
---

On a server that accepts Bedrock players through Geyser, uxmEssentials detects them and renders native
Cumulus forms instead of chest GUIs. It keys off Floodgate, and there is nothing to configure beyond the
Bedrock stack itself.

## What Floodgate Is

- **Geyser** lets Bedrock clients connect to a Java server.
- **Floodgate** is Geyser's companion that identifies which connected players are
  Bedrock, so plugins can treat them appropriately.

uxmEssentials uses Floodgate purely for that **detection**, plus the **Cumulus** form
library Floodgate ships to build native Bedrock UIs.

### Geyser without Floodgate

Running Geyser on its own is supported too. Those servers still carry Bedrock players and
Geyser can name them, so with Floodgate absent and **Geyser-Spigot** present we ask Geyser
instead and Bedrock players still get their native forms.

Floodgate wins whenever both are installed: it is the richer source, and it is the path with
a Bedrock link account behind it. The line the plugin logs on enable names which one answered,
`floodgate`, `geyser` or `none`.

## What uxmEssentials Uses It For

When a Bedrock player opens a menu, the [menu engine](../menus/bedrock.md)
auto-redirects them away from the chest inventory to a native Cumulus form:

- A standard menu degrades automatically into a **SimpleForm** (a scrollable list of
  buttons).
- A menu that declares an explicit `bedrock {}` block renders as a richer
  **CustomForm** (with inputs, toggles, sliders, ...).

Java players on the same server keep getting the normal chest GUI. Both audiences
are served from the same menu definition; you don't maintain two versions.

## Forcing the Chest Path

Some menus rely on real inventory slots (item grids, drag-and-drop). For those, set
`chest-only = true` on the menu so **every** player (Bedrock included) gets the
chest inventory instead of a form.

```
# in a menu spec: keep this one as a chest for everyone
chest-only = true
```

## When Floodgate Is Absent

On a Java-only server, the Bedrock detector loads **none** of the `org.geysermc`
classes: the code path is never touched. Every player is treated as a chest-GUI
viewer and the plugin behaves exactly as it would on any vanilla Paper install.

| Situation | Behavior |
|-----------|----------|
| Floodgate installed, Bedrock player | Native Cumulus form (SimpleForm or CustomForm). |
| Floodgate installed, Java player | Normal chest GUI. |
| No Floodgate (Java-only) | Everyone gets the chest GUI; nothing changes. |

## Setup Checklist

1. Install **Geyser** and **Floodgate** the way you normally would for Bedrock
   support: that's the whole setup.
2. No uxmEssentials config is required; form rendering activates on its own.
3. Add `chest-only = true` to any menu that must use real inventory slots.

Related: [Bedrock Forms](../menus/bedrock.md), [The Custom Menu Engine](../menus/engine.md)
