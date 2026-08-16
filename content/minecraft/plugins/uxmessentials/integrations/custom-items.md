---
title: Custom Items & Heads
order: 1540
description: Using ItemsAdder, Oraxen, Nexo, CraftEngine, MMOItems, ExecutableItems and custom heads as icons.
---

Anywhere uxmEssentials asks for a material you are not limited to vanilla items. A `provider:id` prefix
pulls in custom items from ItemsAdder, Oraxen, Nexo, CraftEngine, MMOItems or ExecutableItems, and custom heads
from HeadDatabase, base64 textures or player skins.

## Custom Item Providers

Prefix the material with the provider name and the item's id:

| Prefix | Provider | Example |
|--------|----------|---------|
| `itemsadder:` | ItemsAdder | `itemsadder:ruby_sword` |
| `oraxen:` | Oraxen | `oraxen:emerald_pickaxe` |
| `nexo:` | Nexo | `nexo:amethyst_axe` |
| `craftengine:` | CraftEngine | `craftengine:default:topaz` |
| `mmoitems:` | MMOItems | `mmoitems:SWORD:cutlass` |
| `ei:` | ExecutableItems | `ei:magic_wand` |

These resolve reflectively, so uxmEssentials never loads a provider's classes unless
that plugin is actually present.

<Callout type="note" title="Exact id format follows the source plugin">

The prefix vocabulary above is fixed, but the part *after* the colon is whatever
id the source plugin expects (ItemsAdder namespaced ids, MMOItems type/id pairs,
...). Copy the id from that plugin's own catalog. CraftEngine takes either form:
`craftengine:default:topaz` names the namespace, `craftengine:topaz` lets
CraftEngine find the path in whichever pack defines it.

</Callout>

## Heads

Custom-head sources use the same prefixed syntax:

| Prefix | Source | Example |
|--------|--------|---------|
| `hdb:` | HeadDatabase, by id | `hdb:12345` |
| `basehead:` | A base64 texture value | `basehead:eyJ0ZXh0dXJlcyI6...` |
| `skull:` | A player's head | `skull:Notch` |

HeadDatabase is guarded strictly: the `hdb:` source only resolves when the plugin
is enabled **and** its API loads. Base64 and player heads are native and need no
extra plugin.

## Where These Work

The `provider:id` syntax is the material spec used by the **menu engine**, which
backs the icon picker across menus and every module GUI. The same shared head
sources (`hdb:` / `basehead:` / `skull:`) also feed hologram head displays and
custom icons on kits and vaults.

```
# a menu item using an ItemsAdder icon
items {
  reward {
    material = "itemsadder:ruby_sword"
    slot = 13
    name = "<gradient:#ff4e50:#f9d423>Ruby Blade</gradient>"
  }
}
```

## When a Provider Is Absent

Custom-item resolution **fails soft**. If a prefix points at a plugin that isn't
installed (or an id that doesn't exist), the lookup silently misses and the renderer
falls back to a plain material: the menu, kit or hologram still opens, just with a
default icon.

| Situation | Behavior |
|-----------|----------|
| Provider installed, id valid | The custom item / head renders. |
| Provider absent or id unknown | Silent fallback to a plain material. |
| HeadDatabase not enabled | `hdb:` is a no-op; `basehead:` / `skull:` still work. |

**Advanced item meta with NBT-API.** For the rare tag that native Paper data-components can't express, uxmEssentials
will use **NBT-API** if it's installed. Item meta is built from native
components first; without NBT-API that raw-NBT path is simply skipped: no
`de.tr7zw` classes are touched.

## Setup Checklist

1. Install whichever item providers you use (**ItemsAdder**, **Oraxen**, **Nexo**,
   **CraftEngine**, **MMOItems**, **ExecutableItems**) and/or **HeadDatabase**: no uxmEssentials
   config needed.
2. Reference items with `provider:id` in menu specs, kit editors and hologram head
   displays.
3. Missing providers degrade gracefully to plain materials, so a menu is never
   broken by an item you haven't installed yet.

Related: [The Custom Menu Engine](../menus/engine.md), [Kits](../modules/kits.md), [Holograms](../modules/holograms.md)
