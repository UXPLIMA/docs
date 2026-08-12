---
title: Welcome
order: 890
---

## What Is uxmEssentials?

uxmEssentials is an **all-in-one server suite for Paper**: one plugin that does
the job of EssentialsX *plus* a dozen add-ons you'd normally bolt on afterwards.

Instead of running a separate plugin for homes, another for warps, another for
your economy, one for kits, one for player vaults, a moderation suite, a hologram
plugin, an NPC plugin, a scoreboard/tablist plugin, a multiverse-style world
manager, and a GUI/menu engine on top; you install **one jar** and turn on the
pieces you want.

Everything a player ever sees: **commands, messages, menus, and per-module
behaviour**: lives in plain text config files. You can rename commands,
re-theme messages, or fully translate the plugin without touching a line of code.

---

## What's In The Box?

| Area | What you get |
|------|--------------|
| 🧭 **Teleports** | `/tpa`, `/back`, `/rtp`, `/spawn`, staff teleports, warmups & cooldowns |
| 🏠 **Homes** | Slot-based, GUI-first personal homes with invites and visiting |
| 🚩 **Warps** | Server warps (cost, lock, password, ratings) + player-owned warps |
| 💰 **Economy** | A DB-backed, multi-currency wallet with `/pay`, `/baltop`, banks and loans |
| 🎁 **Kits** | Claimable kits with cooldowns, previews and an in-game editor |
| 🧰 **Vaults** | Player-owned item storage that survives world rollbacks |
| 🛡️ **Moderation** | Mute, jail, ban, tempban, warn, freeze, with full history and a GUI |
| 🧑‍✈️ **Staff tools** | A dedicated staff mode, staff chat, vanish and gadgets |
| 🪧 **Holograms & NPCs** | Native-display holograms and packet NPCs, both with click action chains |
| 📊 **HUD** | Per-player scoreboard, tablist header/footer, and above-head nametags |
| 🌍 **Worlds** | Create, tune, pregenerate and back up worlds without a second plugin |
| 🎮 **Menu engine** | A data-driven GUI engine that powers every built-in menu, and your own |

...and more: messaging & mail, AFK presence, a PremiumVanish-class vanish, chat control
& announcements, voting rewards, and Discord account linking.

---

## What Makes It Different?

- **Modular.** uxmEssentials is **34 independent feature modules**. Turn any of
  them off and it wires *nothing*: no commands, no listeners, no database
  tables, no background tasks. You never pay for what you don't use.
- **GUI-first.** Homes, kits, warps, vaults, moderation and more open clean
  in-game menus, and *every* module ships with an admin management GUI.
- **Modern config.** Settings live in readable **HOCON `.conf`** files, split
  per module, not one giant `config.yml`.
- **Beautiful text.** All messages use **MiniMessage** formatting and ship in
  more than one language, with a per-player `/lang` override.
- **Multi-currency economy.** Balances are stored in the database (never lost to
  a world rollback) and can run several currencies side by side.
- **Folia-ready & fast.** Built for modern Paper and Folia from day one, with an
  embedded database that needs **zero setup**.

---

## Who Is This Guide For?

| Person | What they'll learn |
|--------|--------------------|
| **Server Owners** | Install the plugin and switch modules on or off |
| **Server Admins** | Configure features, run migrations, and troubleshoot |
| **Players** | Use homes, warps, the economy, kits, vaults and more |

---

## Where Should I Start?

- [🔧 Installation](installation.md): get the plugin running in a couple of minutes
- [🧠 Core Concepts](concepts.md): modules, config, permissions and messages
- [📟 Commands Overview](../modules/index.md): everything you can type in-game
- [🧩 The Module System](../modules/index.md): turn features on and off

---

## Coming From EssentialsX?

You don't have to start from scratch. uxmEssentials ships a one-shot importer that
pulls your homes, warps, balances, kits and more straight across. See
[Migrating from EssentialsX](migration.md).
