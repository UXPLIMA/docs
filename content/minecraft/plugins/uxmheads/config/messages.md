---
title: Messages
order: 503
description: The prefixes and every block of keys with the values each fills in.
icon: message-square
---

Every line comes from `messages/messages_<language>.conf`, MiniMessage with roles instead of colours. A line you delete
falls back to the default built into the plugin.

| Block | Values | Said when |
|---|---|---|
| `admin.version`, `usage`, `reloaded`, `reload-failed` | `<version>`, `<ms>` | The operator commands |
| `admin.doctor.*` | `<check>`, `<said>` | `/uxmheads doctor` |
| `admin.preview.header`, `row` | `<player>`, `<row>` | `/uxmheads preview` |
| `admin.refreshed`, `welcome-sent` | `<player>` | `refresh`, `welcome` |
| `error.not-online` | `<player>` | A player who is not on the server |
| `welcome.line-1` to `line-8` | `<face:1>` to `<face:8>`, `<player>` | Every join |
| `welcome.first.line-1` to `line-8` | the same | A first join |
| `language.*` | `<language>`, `<languages>`, `<player>`, `<input>` | `/uxmheads lang` |
| `command.*` | | The command layer |

The prefix is `<tag:'HEADS'>`; an error is `<etag:'ERROR'>`.

Related: [The welcome](../features/welcome.md), [Languages](languages.md)
