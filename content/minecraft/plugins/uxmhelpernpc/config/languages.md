---
title: Languages
order: 56
icon: languages
---

`language` in `config.yml` chooses a folder:

```
plugins/uxmHelperNPC/languages/
├── en/
│   ├── messages.yml
│   └── menus/
│       ├── npc-main-menu.yml
│       ├── npc-modify-menu.yml
│       ├── glowing-settings-menu.yml
│       └── shop-menu.yml
└── tr/
    └── ...
```

Both messages and menus are per language, so a translation is a folder rather than a patch across
files. Adding a language means copying `en/`, translating it and pointing `language` at the new
folder name.

## Messages

`messages.yml` is MiniMessage throughout. Placeholders arrive as MiniMessage tags rather than
percent-signs — `<remaining_time>` in the cooldown message, `<money>` in the refund message,
`<skin>` in the skin message, `<player_name>` in the admin confirmations.

## Command aliases follow the language

The Turkish subcommands are registered as aliases, always, not only under `language: tr`:
`satınal`, `menü`, `iade`, `getir`, `yoket`. What `language` changes is which set the tab-completer
offers. Both work either way.
