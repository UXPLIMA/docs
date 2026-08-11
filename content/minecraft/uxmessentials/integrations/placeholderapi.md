---
title: PlaceholderAPI
order: 1500
description: 'uxmEssentials works with placeholders in both directions. It publishes its
  own data as placeholders other plugins can display, and it resolves PlaceholderAPI
  tokens inside its own text: menus, holograms, scoreboards and more. If you run
  PlaceholderAPI (and optionally MiniPlaceholders), all of this lights up
  automatically.'
---

---

## What They Are

- **PlaceholderAPI (PAPI)** is the standard bridge that lets plugins share dynamic
  values as `%tokens%`.
- **MiniPlaceholders** does the same for MiniMessage-native `<tags>`, with a focus
  on server-global values.

Both are optional soft-depends.

---

## Placeholders uxmEssentials Provides

When PlaceholderAPI is installed, uxmEssentials registers a
`%uxmessentials_<key>%` expansion covering roughly **25 families** of values: one
per feature area:

| Family | Examples of what it exposes |
|--------|------------------------------|
| economy | balances, formatted amounts, baltop positions |
| homes / warps / vaults | counts and limits |
| teleport | cooldown / warmup state |
| presence / player state | AFK, vanish, playtime, flight, ... |
| moderation | mute / ban status |
| messaging, kits, vote | reply state, kit cooldowns, vote totals |
| server / HUD | TPS, online counts, uptime |

A few confirmed identifiers to show the shape:

```
%uxmessentials_economy_balance%
%uxmessentials_homes_count%
%uxmessentials_server_tps%
```

<Callout type="tip" title="Discover the full list in-game">

The expansion ships with the plugin, so there is no separate download. The
complete, current identifier list is best read live: the placeholders resolve
as soon as PlaceholderAPI is present. Test any of them with
`/papi parse me %uxmessentials_...%`.

</Callout>

---

## Using PAPI Inside uxmEssentials

The reverse direction is just as useful: uxmEssentials resolves **any** installed
PlaceholderAPI expansion inside its own operator-authored text. That means a menu,
hologram or scoreboard can show data from *other* plugins.

- **In menus**: reference a PAPI value with the built-in `%papi_<name>%` token, or
  gate an item with the `papi-compare:` requirement.
- **In holograms & the HUD**: drop `%luckperms_prefix%`, `%server_ram_used%` or any
  other expansion straight into a hologram line, scoreboard row or tablist entry.
- **In the chat format**: the communication module's `chat.conf` format expands
  `%...%` placeholders per speaker, so a rank prefix or balance can sit in the line.

```
# a scoreboard line mixing uxmEssentials and a third-party expansion
Balance: %uxmessentials_economy_balance%
Rank: %luckperms_prefix%
```

---

## MiniPlaceholders

If **MiniPlaceholders** is installed, uxmEssentials folds its **server-global**
`TagResolver` into its own MiniMessage rendering. Operator content can then use
MiniPlaceholders global tags directly:

```
<server_online> players online right now
```

uxmEssentials does **not** register its own MiniPlaceholders expansion; it only
*consumes* the global tags MiniPlaceholders publishes.

---

## When They're Absent

| Missing plugin | Behavior |
|----------------|----------|
| PlaceholderAPI | The `%uxmessentials_%` expansion is never registered; unknown `%tokens%` are left as raw text. HUD keeps working thanks to its **built-in** `{server_*}` tokens, which need no PAPI. |
| A disabled module | Its placeholders degrade to `"-"` rather than erroring. |
| MiniPlaceholders | The global resolver is empty; MiniMessage parses exactly as it would without it. |

---

## Setup Checklist

1. Install **PlaceholderAPI**: the `%uxmessentials_%` expansion registers itself on
   startup, no config required.
2. (Optional) Install **MiniPlaceholders** for `<global_tags>` in your own content.
3. Use `%uxmessentials_...%` in any PAPI-aware plugin, and `%papi_<name>%` /
   `%other_plugin_...%` inside uxmEssentials menus, holograms and the HUD.

---

## Next Steps

- [📊 Scoreboard, Tablist & Nametags](../features/hud.md): where placeholders show up most
- [🎛️ Menu Actions & Requirements](../menus/actions-requirements.md): `%papi_%` tokens and `papi-compare:`
- [🖼️ Holograms](../features/holograms.md): live placeholder lines
