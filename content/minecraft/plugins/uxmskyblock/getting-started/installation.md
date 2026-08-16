---
title: Installation
order: 480
icon: download
---

## Requirements

| Requirement | Minimum |
|-------------|---------|
| **Server** | Paper or Spigot (Paper recommended) |
| **Server Version** | 1.21.4 |
| **Java** | Java 21 or newer |

### Optional dependencies

| Plugin | What it adds |
|--------|--------------|
| **Vault** + an economy plugin | Upgrade costs and the island bank |
| **PlaceholderAPI** | `%skyblock_...%` placeholders for other plugins |
| **FastAsyncWorldEdit** | Schematic-based island creation (`void` worlds) |

<Callout type="note" title="All optional dependencies are soft-depends">

The plugin runs without any of them. Without Vault, upgrades only check the
**level** requirement (money is ignored). Without PlaceholderAPI, menus still
render `%skyblock_...%` placeholders internally.

</Callout>

---

## Step 1: Download

Download the latest `uxmSkyblock-x.x.x.jar`.

## Step 2: Drop It In

Put the jar in your server's `plugins` folder:

```
your-server/
├── plugins/
│   ├── uxmSkyblock-0.4.0.jar   ← put it here
│   └── (other plugins)
├── server.properties
└── paper.jar
```

## Step 3: Start the Server

Start (or restart) the server. On first launch the plugin generates its config
files and the island world.

## Step 4: Check the Config Files

```
plugins/
└── uxmSkyblock/
    ├── config.yml          ← world, commands, storage, proxy, warps
    ├── messages.yml        ← every player-facing message
    ├── roles.yml           ← built-in roles & permissions
    ├── block-values.yml    ← points per block
    ├── levels.yml          ← level thresholds
    ├── upgrades.yml        ← upgrade tiers and costs
    ├── menus/              ← all GUIs (main, settings, upgrades, ...)
    └── modules/            ← drop add-on jars here (e.g. Chunklock)
```

<Callout type="tip" title="Updates never wipe your settings">

When you update the jar, **new keys** from the update are merged into your
existing files automatically; your edited values are kept.

</Callout>

---

## That's It! 🎉

Players can now type `/island` (or `/is`, `/sb`, `/ada`, `/sky`) to open the main
menu and `/is create` to make their first island.

---

## Common Problems

### "Plugin failed to load"

**Cause:** Java is too old.
**Fix:** Install Java 21+. Check with `java -version`.

### Upgrades say I need money but I have plenty

**Cause:** Vault or an economy plugin is missing.
**Fix:** Install Vault + an economy plugin, or set `economy.enabled: false` in
`config.yml` to drop money requirements entirely.

### Islands generate on the ground instead of the void (or vice-versa)

**Cause:** The world generator setting.
**Fix:** Set `world.generator` to `void` (classic empty uxmSkyblock) or `normal`
(real terrain: required by the Chunklock module). See
[config.yml](../configuration/config-yml.md).

---

## What's Next?

- [🏝️ Your First Island](first-island.md)
- [🧠 Core Concepts](concepts.md)
