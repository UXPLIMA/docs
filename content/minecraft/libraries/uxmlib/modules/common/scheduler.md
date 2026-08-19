---
title: Scheduler
order: 211
description: Paper's four schedulers behind one interface, so the same code runs on Paper and Folia.
icon: clock
---

Nothing in uxmLib schedules through `BukkitScheduler`. One `Scheduler` interface covers Paper's four
schedulers, so the same plugin code runs unchanged on Folia.

```java
Scheduler scheduler = new PaperScheduler(plugin);
```

Build it once, in `onEnable`, and inject it. Every uxmLib type that needs scheduling takes one.

## The four families

| Family | Runs on | Use it for |
|---|---|---|
| `global` | The global region | World-wide state: time, weather, a broadcast |
| `region` | The region owning a `Location` | Block and world edits at that location |
| `entity` | The region currently owning an `Entity` | Anything about one entity; it follows the entity across region hops |
| `async` | Off the main threads entirely | I/O and network; never touch the Bukkit API here |

Each family has three methods: run now, run after a delay, and repeat.

```java
scheduler.global(() -> broadcast());
scheduler.globalLater(Duration.ofSeconds(5), () -> announce());
scheduler.globalTimer(Duration.ZERO, Duration.ofMinutes(1), handle -> tick());

scheduler.region(location, () -> place());
scheduler.regionLater(location, Duration.ofSeconds(2), () -> grow());
scheduler.regionTimer(location, Duration.ZERO, Duration.ofSeconds(1), handle -> spread());

scheduler.entity(player, () -> heal());
scheduler.entityLater(player, Duration.ofSeconds(3), () -> reveal());
scheduler.entityTimer(player, Duration.ZERO, Duration.ofSeconds(1), handle -> {
    if (done) handle.cancel();
});

scheduler.async(() -> fetchFromApi());
scheduler.asyncLater(Duration.ofSeconds(10), () -> retry());
scheduler.asyncTimer(Duration.ZERO, Duration.ofMinutes(5), handle -> poll());
```

Delays and periods are `Duration`s, rounded to whole ticks for the tick-based schedulers and to
milliseconds for the async one.

## Task handles

Every method returns a `TaskHandle`:

```java
TaskHandle handle = scheduler.globalTimer(Duration.ZERO, Duration.ofSeconds(1), h -> tick());
handle.cancel();
handle.isCancelled();
```

Timer variants hand the task its own handle, so a repeating task can stop itself without a field
holding a reference to something that does not exist yet.

<Callout type="warning" title="The abstraction makes the threading model visible, not optional">

Folia's threading model is the reason four schedulers exist. Touching a block from the wrong region
is still a bug; the abstraction gives you a place to be right rather than preventing you from being
wrong. Pick the family that owns the state you are about to touch.

</Callout>

An entity task is silently dropped if the entity has been removed by the time it would run, which is
the behaviour you want for anything keyed to a player who may have logged out.

## Ticks

```java
Ticks.ONE_TICK;                              // Duration of one tick
Ticks.fromDuration(Duration.ofSeconds(3));   // 60
```

## The tick clock

A shared counter of elapsed ticks, for anything that animates on a period rather than a wall clock.

```java
TickClock clock = new TickClock(scheduler);
clock.start();
long now = clock.ticks();
clock.stop();
```

The GUI module uses one internally to drive animated items and slot animations, which is why those
features need the `Scheduler` overload of `Guis.install`.

## Testing against it

`Scheduler` is an interface with twelve methods and no server dependency in its signature, so a test
can supply a same-thread implementation that runs each task inline and assert on what happened,
without standing a server up. That seam is why the rest of the library can take a scheduler and still
be tested.
