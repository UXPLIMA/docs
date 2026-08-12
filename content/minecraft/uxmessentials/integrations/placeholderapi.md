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

When PlaceholderAPI is installed, uxmEssentials registers a `%uxmessentials_<key>%`
expansion carrying **170 keys** across 22 areas: economy, homes, warps, player warps,
worlds, teleport, kits, vaults, vote, presence, player state, messaging, communication,
moderation, staff, Discord linking, ranks, poses, holograms, menus, the scoreboard and
the server metrics.

```
%uxmessentials_economy_balance%
%uxmessentials_homes_count%
%uxmessentials_presence_afk%
%uxmessentials_server_tps%
```

Every key, with what it renders and whether it answers for an offline player, is on the
[Placeholder Reference](../placeholders/reference.md). That page is generated from the
catalogue inside the plugin, and a build-time guard resolves the catalogue against the
resolver in both directions, so it cannot name a key this build does not answer.

### The Two-Player Form

A handful of keys read the relation between two players rather than one player's own
state: whether the viewer can see the other, ignores them, is trading with them, and how
far apart they stand. These carry PlaceholderAPI's `rel_` prefix and only answer on a
surface that renders per viewer, such as a chat format, a tab line or a nametag.

```
%rel_uxmessentials_cansee%
%rel_uxmessentials_ignoring%
%rel_uxmessentials_distance%
%rel_uxmessentials_trading%
```

Anywhere else, PlaceholderAPI has only one player to work with and leaves the raw token
in place, which is how you tell "no relation" from "wrong surface".

<Callout type="tip" title="Read the list from the running server">

`/uxmess placeholders` lists the areas in game and `/uxmess placeholders <area>` reads
one; `/uxmess placeholders export` writes the whole catalogue to `placeholders.md` in
the plugin folder. Test any single key with `/papi parse me %uxmessentials_...%`.

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

- [📊 Scoreboard, Tablist & Nametags](../modules/scoreboard.md): where placeholders show up most
- [🎛️ Menu Actions & Requirements](../menus/actions-requirements.md): `%papi_%` tokens and `papi-compare:`
- [🖼️ Holograms](../modules/holograms.md): live placeholder lines
- [🔤 Placeholder Reference](../placeholders/reference.md): every key this build answers
