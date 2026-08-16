---
title: UI Style
order: 1190
description: The palette, glyphs and text formats the built-in messages and menus follow.
icon: palette
---

uxmEssentials ships with one consistent look across chat, command feedback, GUIs and the
HUD. When you edit [messages](messages.md) or build [custom menus](../menus/engine.md),
following the same conventions keeps your server looking like one plugin instead of a
patchwork. This page is the practical summary for operators.

**HOCON, not YAML.** Message catalogs and menu files are HOCON: quoted `"key" = "value"` lines and
`{ … }` blocks. The styling below is written in MiniMessage tags inside those values.

## Style tokens, not raw colours

The plugin defines a fixed palette once, in code, and exposes it as **semantic tags**.
Instead of writing a hex code, you name the *role* the text plays. This is why editing one
message never clashes with the rest of the UI: every line pulls from the same palette.

| Token | Use it for | Colour |
|---|---|---|
| `<value>` / `<accent>` | Names, `{placeholder}` values, coordinates | cyan |
| `<body>` | The sentence itself, field labels: the default reading colour | cream |
| `<cta>` | "Click to…", `/command` hints: the next action | orange |
| `<money>` / `<good>` | Money amounts and success words | green |
| `<bad>` | Error / denial wording | red |
| `<level>` | Counts, page numbers, quantities | yellow |
| `<muted>` | Dates, breadcrumbs, list bullets, parentheticals | gray |
| `<tag:'X'>` | The chat prefix for a normal message (`X` = module label) | gold gradient |
| `<etag:'X'>` | The chat prefix for an error/denial | red gradient |
| `<h:'X'>` | A gold-gradient bold header: GUI titles and lore titles | gold gradient |

The solid-colour tokens wrap their content (`<value>Steve</value>`). The composite tokens
(`<tag:'HOME'>`, `<etag:'ERROR'>`, `<h:'Your Homes'>`) take one quoted argument and are
self-closing.

<Callout type="danger" title="Never inline hex or legacy codes">

Don't write `<#7cc7ff>`, `<gradient:#…>`, `&a`, or `§a` in a restyled line. Reference
a token instead. A raw hex in a catalog is treated as a regression and flagged by the
build's drift checks.

Also never put a `{placeholder}` **inside** a tag argument (e.g. `<h:'Home {home}'>`):
a value containing a quote or angle bracket would break the tag. Put dynamic values
outside the argument: `<h:'Home'> <value>{home}</value>`.

</Callout>

## The line shapes

**Chat / feedback.** Lead with a prefix, colour the parts by role, end with a status
glyph where it reads well:

```hocon
"pay.sent" = "<tag:'ECONOMY'> You paid <money>{amount}</money> to <value>{player}</value>. 💰"
"command.no-permission" = "<etag:'ERROR'> <bad>You don't have permission.</bad> ✗"
```

Use `<tag:'MODULE'>` for normal messages and `<etag:'MODULE'>` for errors and denials,
where `MODULE` is the feature label (`HOME`, `WARP`, `ECONOMY`, …).

**GUI item lore.** Descriptive lore follows a fixed beat, written as one `<newline>`-joined
string: a crest + title, a muted breadcrumb, a section header, body lines, then the action.

```hocon
" ⛨ <h:'Home Panel'><newline>    <muted>Your saved homes</muted><newline><newline> ✎ <accent>Description</accent><newline>   <body>Teleport to a saved location instantly.</body><newline><newline> ▶ <cta>Click to teleport</cta>"
```

A labelled fact inside lore is one info row: a muted `|` bullet, a `<body>` label, the
value, and a matching glyph:

```hocon
"<muted>|</muted> <body>Balance:</body> <money>{bal}</money> 💰"
```

## Glyph legend

These are plain Unicode characters (no resource pack). Use each for its meaning:

| Glyph | Meaning |
|---|---|
| `▶` | Action / "click to…" |
| `✎` | Section header inside lore |
| `⛨` | Panel / module crest (lore title line) |
| `ℹ` | Neutral information |
| `🧭` | Location, world, teleport |
| `💰` | Money, balance, payment |
| `🏆` | Ranking, leaderboard |
| `👑` | Ownership |
| `📅` | A date or time |
| `👥` | People / members |
| `✓` / `✗` | Success / denial |

## Styling menus

Custom menus in `plugins/uxmEssentials/menus/*.conf` are styled with the **same
MiniMessage tags**. Titles, item names, and lore in a menu file are written **verbatim**:
they are not routed through the message catalogs, so you can style them however you like,
though matching the palette above keeps them consistent with the built-in GUIs.

```hocon
# menus/example.conf
title = "<h:'Example Menu'>"
rows = 3
items {
  spawn {
    slot = 11
    material = COMPASS
    name = "<cta>Go to spawn</cta>"
    lore = [" ▶ <cta>Click to teleport</cta>"]
    click { left = ["command:spawn", "close"] }
  }
}
```

The built-in module GUIs (the `modules/<m>/gui/*.conf` panels) already follow these
conventions: treat them as worked examples if you are building your own.

## A few rules of thumb

- **Do** reference colours by token, use `<h:'…'>` for headers, and end chat lines with
  the matching status glyph.
- **Do** use normal capitalization: Title Case for headers, sentence case for body text.
- **Don't** inline hex or legacy `&`/`§` codes, invent new tokens, or rename
  `{placeholders}`.
- Styling is language-independent: when you restyle a key in one language, apply the same
  token edit to the matching key in every other language file.

Related: [Messages & Languages](messages.md), [Custom Menu Engine](../menus/engine.md), [Built-in Menu Guide](../menus/built-in.md)
