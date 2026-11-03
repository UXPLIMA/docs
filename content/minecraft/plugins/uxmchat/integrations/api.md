---
title: API
order: 805
description: Reaching uxmChat from another plugin, and the events it fires.
icon: code
---

Other plugins reach `com.uxplima.chat.api.UxmChat` through Bukkit's services manager:

```java
UxmChat chat = Bukkit.getServicesManager().load(UxmChat.class);
```

and listen to the events in `com.uxplima.chat.api.event`:

| Event | Fired when | Cancellable |
|---|---|---|
| `ChatLineEvent` | A player's line is about to go out | yes |
| `ChatLineSentEvent` | A line went out | no |
| `ChatMentionEvent` | A player was mentioned | no |
| `PrivateMessageEvent` | A private message is sent | no |
| `ServerLineEvent` | A server line (join, quit, death...) is said | no |
| `StaffActionEvent` | A staff action is recorded: mute, delete, clear, lock | no |

Depend on uxmChat in your `paper-plugin.yml` with `load: BEFORE` and `join-classpath: true` to see the API classes.

Related: [Integrations](overview.md)
