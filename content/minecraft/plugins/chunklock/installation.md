---
title: Installation
order: 1670
description: Chunklock is a module, so installing it means dropping a jar into uxmSkyblock
  and setting a couple of core options.
icon: download
---

---

## Requirements

| Requirement | Notes |
|-------------|-------|
| **uxmSkyblock** | Installed and running (see its [installation](../uxmskyblock/getting-started/installation.md)) |
| `world.generator: normal` | Chunklock needs **real terrain**, not the void |
| `border.enabled: false` | Chunklock manages its own border |

---

## Step 1: Drop In the Module

Place the jar in the core's **modules** folder:

```
plugins/
└── uxmSkyblock/
    └── modules/
        └── Chunklock-1.0.0.jar   ← here
```

## Step 2: Set the Core Options

In `plugins/uxmSkyblock/config.yml`:

```yaml
world:
  generator: normal      # real terrain so locked chunks reveal natural ground

border:
  enabled: false         # Chunklock manages its own WorldBorder
```

<Callout type="warning" title="Two borders fighting">

If you leave the core `border.enabled: true`, you'll have two WorldBorders set
on the same player at once. Always set it to `false` when Chunklock is active.

</Callout>

## Step 3: Restart

Restart the server. The core loads the module after its own startup; you'll see
Chunklock's config generated at:

```
plugins/uxmSkyblock/modules/Chunklock/config.yml
```

---

## Optional: A Pure Chunklock Server

To run a server that hosts **only** Chunklock (no normal `/ada` islands menu),
set in the core config:

```yaml
island:
  enabled: false
```

The `/island` command and island world stay off, but the core managers (economy,
block values, storage, proxy, module loader) keep running so Chunklock still works.

<Callout type="note">

Even with `island.enabled: false`, Chunklock still creates uxmSkyblock islands
under the hood: that's how it reuses teams, roles, and warps. The flag only
controls whether the standalone island UI/world is registered.

</Callout>

---

## Next

- [How It Works](how-it-works.md)
- [Configuration](configuration.md)
