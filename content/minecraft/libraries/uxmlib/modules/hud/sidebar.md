---
title: Sidebar
order: 271
description: A per-player scoreboard sidebar that only re-sends the lines that actually changed.
icon: panel-right
---

Most sidebar implementations clear and rebuild every update, which the client renders as a flicker.
This one diffs.

```java
SidebarManager sidebars = new SidebarManager(Bukkit.getScoreboardManager());
SidebarManager withScheduler = new SidebarManager(Bukkit.getScoreboardManager(), scheduler);

Sidebar sidebar = sidebars.create(player, Text.mini("<gold><bold>Server"));

sidebar.lines(List.of(
        Text.mini("<gray>Online: <white>42"),
        Text.mini("<gray>Map: <white>spawn"),
        Text.mini("<gray>Balance: <gold>1,250")));

sidebar.show();
```

Call `lines` again with the new content on every update. Only the lines whose text differs are
touched, so the client never sees a clear and rebuild.

| Method | Effect |
|---|---|
| `title(Component)` | Replace the title |
| `lines(List<Component>)` | Replace the content, diffed |
| `show()` | Display it to the player |
| `remove()` | Take it down |
| `currentLines()` | What is shown now |
| `player()` | The viewer |

`Sidebar.MAX_LINES` is 15, bounded by the invisible-key space each line uses, which is the practical
sidebar height anyway.

## The manager

```java
sidebars.get(uuid);
sidebars.remove(player);
sidebars.forget(uuid);        // on quit
sidebars.count();
```

```java
sidebars.showTemporary(player, title, lines, Duration.ofSeconds(10));
```

A sidebar that takes itself down, for an event countdown or a one-off notice. It needs the
`Scheduler` overload of the manager.

## Playing well with other plugins

```java
sidebars.onBoardSwitch((player, board) -> logger.info("something replaced our board"));
Bukkit.getPluginManager().registerEvents(new SidebarListener(sidebars), plugin);
```

Another plugin setting a player's scoreboard silently replaces yours. `SidebarListener` notices, and
the callback is your chance to react rather than to keep writing lines nobody sees.

## How a line works

Each line is a team whose entry is a fixed, unique, invisible colour-code key, and whose visible text
is the team prefix. That is what makes an update a prefix change rather than an entry rewrite, and it
is why the line count is bounded by the colour-code key space.
