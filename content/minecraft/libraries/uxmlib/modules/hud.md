---
title: uxmlib-hud
order: 27
icon: monitor
---

Overlays delivered through Paper's own player API — no packets, no NMS.

## Sidebar

```java
SidebarManager sidebars = new SidebarManager(Bukkit.getScoreboardManager());

Sidebar sb = sidebars.create(player, Text.mini("<gold><bold>Server"));
sb.lines(List.of(
        Text.mini("<gray>Online: <white>42"),
        Text.mini("<gray>Map: <white>spawn")));
sb.show();
```

`sb.lines(...)` diffs against what the player currently has and re-sends only the lines that
changed. That is what makes it flicker-free: the usual approach clears and rewrites the whole
objective every tick, and the client renders the gap.

Call it every tick with the same content and nothing is sent at all.

## Titles

```java
new Titles().show(player, Text.mini("<green>Welcome"), Text.mini("<gray>have fun"));
```

Title and subtitle, with the usual fade and stay timings.

## Action bar

```java
new ActionBarManager(scheduler, server)
        .show(player, Text.mini("<yellow>Saved!"), Duration.ofSeconds(3));
```

Sticky: the action bar clears itself after a couple of seconds, so a message that should stay for
three has to be resent. The manager does that, and takes a duration instead.

It also arbitrates between callers — two features writing to the action bar at once produce a
flicker otherwise.

## Boss bars

```java
new BossBarManager(scheduler, server)
        .countdown(player, Text.mini("<red>Event"), Duration.ofMinutes(1));
```

`BossBarMode` covers the four shapes:

| Mode | Behaviour |
|---|---|
| `PERMANENT` | Stays until removed |
| `FILLING` | Progress from empty to full |
| `COUNTDOWN` | Full to empty over a duration |
| `DYNAMIC` | Progress you set |

`RemainingTime` formats the countdown text.

## Tablist

```java
new Tablist().set(player, header, footer);
```

Header and footer, per player.

## Animation

`TextAnimator` drives per-tick text effects, with `GradientText` and `ScrollingText` as the built-in
ones. `UpdateBatch` groups several HUD changes so they are sent together rather than as separate
packets in the same tick.

## Everything is per-player

Every type here takes a `Player`. There is no server-wide scoreboard state and no shared objective —
which means two plugins can each show a sidebar without fighting, and a per-player value needs no
special handling.

The cost is that a server-wide bar is a loop over players. `UpdateBatch` is there for exactly that.
