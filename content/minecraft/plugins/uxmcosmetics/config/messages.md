---
title: Messages
order: 607
description: How a line is written, the prefixes, and every block of keys with the values each fills in.
icon: message-square
---

Every line comes from `messages/messages_<language>.conf`. No text is written in the code. Run `/uxmcosmetics reload`
after an edit.

```hocon
equip {
  done = "<tag:'COSMETICS'> <body>You are now wearing <value><name><body>."
}
```

- A line is MiniMessage and names **roles**, not colours: `<body>`, `<value>`, `<money>`, `<good>`, `<bad>`, `<cta>`,
  `<subtext>`, `<muted>`. The colours are in [theme.conf](theme.md).
- A tag that is not a role is a **value** the plugin fills in, such as `<name>`.
- Write ordinary letters: small capitals are applied when drawn, for the languages that use them.
- A line you delete falls back to the default built into the plugin.

| Prefix | Draws |
|---|---|
| `<tag:'COSMETICS'>` | The plugin's category word |
| `<tag:'MONEY'>` | A purchase or a refund |
| `<tag:'GADGET'>` | A gadget's answer |
| `<etag:'ERROR'>` | An error |
| `<h:'...'>` | A header |

## The keys

| Block | Values | Said when |
|---|---|---|
| `admin.version`, `usage`, `reloaded`, `reload-failed` | `<version>`, `<count>`, `<ms>` | The plugin commands |
| `admin.granted`, `grant-already`, `revoked`, `revoke-not-owned`, `cleaned` | `<name>`, `<player>`, `<count>` | Grant, revoke, cleanup |
| `admin.station.*` | `<id>`, `<box>`, `<x>`, `<y>`, `<z>`, `<world>`, `<piece>`, `<setting>`, `<written>` | Station commands |
| `admin.doctor.*` | `<check>`, `<said>` | `/uxmcosmetics doctor` |
| `error.*` | `<id>` | Unknown piece, box or player; a failed write (`busy`) |
| `equip.*`, `unequip.*` | `<name>`, `<category>`, `<count>` | Wearing |
| `buy.*` | `<name>`, `<price>`, `<balance>` | Buying |
| `box.*` | `<name>`, `<coins>`, `<price>`, `<balance>` | Opening a box |
| `burst.*` | `<category>`, `<seconds>`, `<name>` | Playing an emote or gadget |
| `station.all-boxes`, `station.sign.open` | | A station's sign |
| `menu.*` | the tokens | Every window |
| `opener.*` | | The opener item |
| `gadget.lore`, `cooldown-*`, `no-room`, `says`, `magic-ball.1`-`8`, `fortune.1`-`5` | `<seconds>`, `<line>` | Gadget items and answers |
| `category.*`, `category-description.*` | | Family names and sentences |
| `rarity.*` | | Rarity words with their colours |
| `command.*` | | The command layer: help pages, rejected arguments |
| `language.*` | `<language>`, `<languages>`, `<player>`, `<input>` | `/cosmetics lang` |
| `cosmetic.<family>.<id>.name`, `.description` | | Every shipped piece |

Related: [Languages](languages.md), [theme.conf](theme.md)
