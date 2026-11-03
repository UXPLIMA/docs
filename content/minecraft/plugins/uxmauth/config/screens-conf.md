---
title: screens.conf
order: 606
description: The waiting room keys and the label keys of every dialog.
icon: app-window
---

| Key | Default | Meaning |
|---|---|---|
| `waiting-room` | `"dialog"` | `dialog`, `frozen` or `login-world` |
| `login-world-name` | `"uxmauth_login"` | |
| `cancel` | `"kick"` | `kick` or `world` |
| `can-close` | `false` | |
| `chat-input` | `false` | |
| `pin-pad` | `false` | |
| `rules.enabled`, `rules.lines` | `false`, two `@rules` lines | |

Each screen block, `register`, `login`, `replace`, `captcha`, `code`, `recover` and `pin`, holds a `title`, a `body` list
and one key per field or button, each an `@key` of the messages folder or your own MiniMessage:

| Screen | Keys |
|---|---|
| `register` | `title`, `body`, `password`, `confirm`, `accept`, `remember`, `submit`, `cancel` |
| `login` | `title`, `body`, `password`, `remember`, `submit`, `claim`, `forgot`, `cancel` |
| `replace` | `title`, `body`, `password`, `confirm`, `submit`, `cancel` |
| `captcha` | `title`, `body`, `code`, `submit`, `cancel` |
| `code` | `title`, `body`, `code`, `submit`, `cancel` |
| `recover` | `title`, `body`, `code`, `password`, `submit`, `cancel` |
| `pin` | `title`, `body`, `erase`, `enter`, `cancel`, `remember`, `entered`, `repeat`, `mask` |

An empty `remember`, `claim` or `forgot` takes that box or button off.

Related: [Screens](../menus/screens.md), [The waiting room](../features/waiting-room.md)
