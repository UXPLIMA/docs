---
title: Animation
order: 273
description: Per-tick text animators and a batcher that collapses many updates into one render.
icon: film
---

## Text animators

A `TextAnimator` produces one frame at a time. Advance it on a timer and write the frame wherever it
belongs: a sidebar title, a boss bar name, a hologram line.

```java
GradientText gradient = GradientText.of("UXPLIMA", List.of(TextColor.color(0xFF5555), TextColor.color(0xFFAA00)));
GradientText slower = GradientText.of("UXPLIMA", stops, 40);   // 40 steps per cycle

ScrollingText scroll = ScrollingText.of("A long announcement that will not fit", 20);
ScrollingText spaced = ScrollingText.of(text, 20, "   ***   ");
```

```java
scheduler.globalTimer(Duration.ZERO, Duration.ofMillis(100), handle -> {
    sidebar.title(gradient.frame());
    gradient.advance();
});
```

| Method | Effect |
|---|---|
| `frame()` | The current frame as a component |
| `advance()` | Move to the next frame |
| `reset()` | Back to the start |

`GradientText` sweeps a gradient across fixed text. `ScrollingText` slides long text through a fixed
window, with a separator between the end and the start of the loop.

Both are pure: no server, no scheduler, no player. They produce components and you decide where they
go, which is why the same animator drives a sidebar title and a hologram line without adaptation.

## Update batching

Many updates for the same surface within one tick should be one render.

```java
UpdateBatch<UUID> batch = new UpdateBatch<>(scheduler, uuid -> renderSidebar(uuid));

batch.mark(uuid);      // called from anywhere something changed
batch.isArmed();
```

The first `mark` after an idle period arms one flush a tick out. Further marks before it fires just
add to the dirty set. When the flush runs, each dirty key is rendered once, in first-marked order.

This is the difference between a sidebar that re-renders forty times because forty things changed in
one tick, and one that renders once.

A re-entrancy guard stops a renderer re-dirtying the key it is currently rendering, so there is no
feedback loop. A mark for a **different** key made from inside a render is still honoured and lands
in the next flush.

<Callout type="warning" title="Drive it from one thread">

`UpdateBatch` is not thread-safe. Mark from a single scheduler thread, or marshal onto one first.

</Callout>
