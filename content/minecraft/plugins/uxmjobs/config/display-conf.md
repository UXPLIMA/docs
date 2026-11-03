---
title: display.conf
order: 604
description: Every key of display.conf with its default.
icon: monitor
---

| Key | Default |
|---|---|
| `action-bar.enabled`, `default-on`, `linger-seconds` | true, true, 3 |
| `boss-bar.enabled`, `default-on`, `linger-seconds` | true, false, 5 |
| `chat-prefix.enabled` | false |
| `dialogs.enabled` | true |
| `level-up.sound` | `"entity.player.levelup"` |
| `level-up.firework`, `firework-colour`, `firework-shape` | true, `"#4ecca3"`, `"ball"` |
| `level-up.broadcast-from-level` | 25 |
| `level-up.actions` | `[]` |
| `paid.actions` | `[]` |
| `paid.empty-account-notice-seconds` | 60 |

A firework colour or shape the server cannot read falls back to the shipped value and says so in the console. What each
does is on [Display](../features/display.md).

Related: [Display](../features/display.md)
