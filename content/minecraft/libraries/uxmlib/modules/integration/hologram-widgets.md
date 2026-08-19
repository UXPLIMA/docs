---
title: Hologram widgets
order: 263
description: Paged, conditional and live-leaderboard holograms, each showing a different thing to each player.
icon: layout-dashboard
---

The base hologram shows one thing. These three show a different thing to each viewer, using Paper's
own per-viewer `show` and `hide` rather than a packet layer.

All three implement `HologramLifecycle`, so registering them with the `HologramManager` resets a
player's state on quit and world change with no listener of your own.

```java
holograms.registerLifecycle(widget);
holograms.unregisterLifecycle(widget);
```

## Paged

N pages of content live as N overlapping holograms at one location, and each player sees exactly one.

```java
PagePresenter presenter = new DisplayPagePresenter(pageHolograms, plugin, scheduler);
PagedHologram paged = new PagedHologram(pageHolograms.size(), presenter);

paged.open(viewer);
paged.next(viewer);
paged.prev(viewer);
paged.pageCount();
```

Paging hides the page the viewer is on and shows the one they move to, for that viewer only, so it
never disturbs anyone else standing there.

The page arithmetic is a pure `PageState` and the show and hide go through a `PagePresenter`, so the
widget is testable with no live entity.

## Conditional

The same idea generalised from an index to a condition. Several overlapping states, and each viewer
sees whichever condition passes first for them. This is the hologram equivalent of a stateful GUI
item.

```java
SwitchSelection<Hologram> selection = new SwitchSelection<>(List.of(
        new SwitchSelection.State<>(ctx -> ctx.stat("balance") >= 10_000, richHologram),
        new SwitchSelection.State<>(ctx -> true, defaultHologram)));

SwitchableHologram<Hologram> switchable = new SwitchableHologram<>(
        selection, new DisplayStatePresenter<>(holograms, plugin, scheduler));

switchable.refresh(new ViewerContext(uuid, key -> statOf(uuid, key)));
switchable.shownState(uuid);
```

`ViewerContext` is the viewer's UUID plus a stat lookup, so a state can switch on a permission tier,
a balance or a score without the widget knowing what any of those are.

Order the states most specific first, with a `true` fallback last, exactly as with a stateful GUI
item.

## Live leaderboard

```java
LeaderboardOptions options = LeaderboardOptions.topN(10)
        .descending(true)
        .format("<gray><rank>. <white><name> <dark_gray>- <gold><score>")
        .topFormats(List.of(
                "<gold><rank>. <yellow><name> <dark_gray>- <gold><score>",
                "<gray><rank>. <white><name> <dark_gray>- <gold><score>"))
        .header(Text.mini("<gold><bold>Top players"))
        .emptyLine("<dark_gray>...")
        .locale(Locale.ENGLISH);

LeaderboardRenderer renderer = new LeaderboardRenderer(options, uuid -> nameOf(uuid));

LeaderboardHologram board = new LeaderboardHologram(
        hologram, renderer, () -> loadScores(), scheduler, Duration.ofSeconds(30));

board.start();
board.stop();
board.isRunning();
```

Each tick pulls the latest scores, renders them to lines, and writes them back onto the hologram on
the entity's own region thread.

`topFormats` lets the first few places use a different format from the rest, which is how a
leaderboard gets a gold first place without special-casing in your own code. `emptyLine` fills the
positions that have no entry yet, so the board does not change height as it fills up.

### The nearby gate

```java
new LeaderboardHologram(hologram, renderer, scores, scheduler, period,
        () -> anyPlayerWithin(location, 48));
```

Checked first each tick. When it returns false, the tick skips the data pull and the re-render
entirely, so a leaderboard in an empty corner of the map costs nothing.

You supply the check, which is usually a cheap loop over online players. The widget never reads
positions itself, which is what keeps it unit-testable.

`LeaderboardRenderer` is pure and usable on its own:

```java
List<Component> lines = renderer.render(scores);
```

That is the form to use when the leaderboard should be a sidebar or a menu rather than a hologram.
