---
title: uxmlib-hud
order: 27
description: Sidebar, titles, action bar, boss bars, tablist and text animators, all through the public player API.
icon: monitor
---

Adventure-native HUD overlays delivered through Paper's own player API. No packets and no NMS
anywhere in this module.

| Page | Covers |
|---|---|
| [Sidebar](sidebar.md) | The flicker-free diffing scoreboard sidebar |
| [Bars and titles](bars.md) | Titles, the sticky action bar, boss bars, tablist |
| [Animation](animation.md) | Gradient and scrolling text animators, update batching |

## The short version

```java
SidebarManager sidebars = new SidebarManager(Bukkit.getScoreboardManager());
Sidebar sb = sidebars.create(player, Text.mini("<gold><bold>Server"));
sb.lines(List.of(Text.mini("<gray>Online: <white>42")));
sb.show();

new Titles().show(player, Text.mini("<green>Welcome"), Text.mini("<gray>have fun"));
new Tablist().set(player, header, footer);

new ActionBarManager(scheduler, server).show(player, Text.mini("<yellow>Saved"), Duration.ofSeconds(3));
new BossBarManager(scheduler, server).countdown(player, Text.mini("<red>Event"), Duration.ofMinutes(1));
```

Each manager holds its state on the instance, never statically, so two plugins each with their own
manager do not interfere.
