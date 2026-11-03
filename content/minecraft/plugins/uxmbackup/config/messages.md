---
title: Messages
order: 604
description: The prefixes and every block of keys with the values each fills in.
icon: message-square
---

Every line comes from `messages/messages_<language>.conf`, MiniMessage with roles instead of colours.

| Block | Values | Said when |
|---|---|---|
| `admin.*` | `<version>`, `<ms>`, `<check>`, `<said>` | Version, reload, doctor |
| `error.*` | `<id>` | Unknown names |
| `run.*` | `<set>`, `<id>`, `<size>`, `<took>`, `<detail>` | `/uxmbackup run` |
| `list.*` | `<count>`, `<id>`, `<state>`, `<size>`, `<when>` | `/uxmbackup list` |
| `info.*` | `<id>`, `<name>`, `<detail>` | `/uxmbackup info` |
| `verify.*` | `<id>`, `<checked>`, `<missing>`, `<damaged>` | `/uxmbackup verify` |
| `prune.*` | `<storage>`, `<removed>`, `<kept>`, `<detail>` | `/uxmbackup prune` |
| `storages.*` | `<id>`, `<reach>`, `<count>`, `<size>` | `/uxmbackup storages` |
| `status.*` | `<name>`, `<detail>`, `<id>`, `<since>` | `/uxmbackup status` |
| `restore.*` | `<id>`, `<count>`, `<kind>`, `<target>`, `<size>`, `<minutes>`, `<name>`, `<folder>`, `<hash>`, `<detail>` | Every restore step and refusal |
| `menu.*` | `<id>`, `<set>`, `<size>`, `<when>`, `<storages>` | The archives window |
| `command.*` | | The command layer, including `turned-off` |
| `language.*` | `<language>`, `<languages>`, `<player>`, `<input>` | `/uxmbackup lang` |
| `words.*` | | The words a run's state and storage reach are written in |

The prefixes are `<tag:'BACKUP'>`, `<tag:'WARNING'>` and `<etag:'ERROR'>`.

Related: [Languages](languages.md)
