---
title: Bars and titles
order: 272
description: Titles, a sticky action bar, boss bars with four progress modes, and the tablist.
icon: gauge
---

## Titles

```java
Titles titles = new Titles();

titles.show(player, Text.mini("<green>Welcome"), Text.mini("<gray>have fun"));
titles.show(player, title, subtitle, fadeIn, stay, fadeOut);
titles.clear(player);
titles.reset(player);
```

`clear` hides the current title; `reset` also restores the default timings.

## Tablist

```java
Tablist tablist = new Tablist();

tablist.set(player, header, footer);
tablist.header(player, header);
tablist.footer(player, footer);
tablist.clear(player);
```

## The action bar

Vanilla fades an action-bar message after about two seconds. A message that should sit there for ten
has to be re-sent.

```java
ActionBarManager bars = new ActionBarManager(scheduler, server);

bars.show(player, Text.mini("<yellow>Saved"), Duration.ofSeconds(10));
bars.countdown(player, "<red>Respawning in <time>", Duration.ofSeconds(5));
bars.clear(uuid);
bars.tracked();
bars.close();
```

One shared repeating task re-sends each tracked player's message until its deadline, at an interval
well under the fade window so the line never visibly blinks. The task starts when the first player is
tracked and cancels itself once the last entry expires, so an idle server runs no task.

`countdown` takes a template containing a `<time>` tag, filled from the remaining duration each tick.

## Boss bars

```java
BossBarManager bars = new BossBarManager(scheduler, server);

BossBar bar = bars.countdown(player, Text.mini("<red>Event starting"), Duration.ofMinutes(1));
BossBar templated = bars.countdown(player, "<red>Starting in <time>", Duration.ofMinutes(1));

bars.show(player, BossBar.bossBar(name, 1f, RED, PROGRESS), BossBarMode.FILLING, Duration.ofSeconds(30));

BossBar live = bars.dynamic(player, Text.mini("<gold>Health"), p -> p.getHealth() / 20f);
BossBar both = bars.dynamic(player,
        p -> Text.mini("<gold>" + p.getName()),
        p -> p.getHealth() / 20f);

bars.hide(uuid);
bars.barOf(uuid);
bars.tracked();
bars.close();
```

| Mode | Progress |
|---|---|
| `PERMANENT` | Never changes on its own; you set it |
| `FILLING` | Ramps 0 to 1 over the duration, then holds full |
| `COUNTDOWN` | Ramps 1 to 0 over the duration, then auto-hides |
| `DYNAMIC` | Re-evaluated from your function every tick; the duration is ignored |

The filling and countdown ramps are pure arithmetic on the enum, so they are unit-testable with no
Bukkit plumbing at all.

## Remaining-time placeholders

```java
TagResolver time = RemainingTime.resolver(() -> Duration.between(now(), deadline));
TagResolver named = RemainingTime.resolver("left", () -> remaining());

viewer.sendMessage(Text.mini("<gray>Ends in <time>", time));
```

The default resolver answers both `<time>` and the `<auto_time_left>` alias. The two-argument form
names a single key of your own, which is what you need when one message carries two different
countdowns.

The supplier is read lazily, once per render and only if the tag actually appears in the template, so
a title that omits it pays nothing. The value is formatted through `Durations.format`, so a countdown
reads in the same vocabulary as a cooldown or a ban.

## Cleaning up

```java
Bukkit.getPluginManager().registerEvents(new HudListener(bossBars, actionBars), plugin);
```

Drops a player's tracked bars when they quit, so nothing leaks across a reconnect.

Call `close()` on both managers in `onDisable`.
