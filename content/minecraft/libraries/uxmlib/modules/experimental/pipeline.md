---
title: Pipeline
order: 311
description: Resolving a player's channel, injecting an interceptor idempotently, and re-asserting position when another plugin splices ahead.
icon: git-merge
---

`uxmlib-npc` is the foundation the rest of the packet layer sits on. It is Netty plumbing, not NPCs.

## Injecting

```java
PacketPipeline pipeline = new PacketPipeline(resolver, registry, "myplugin", watchdog);

pipeline.inject(player);
pipeline.isInjected(player);
pipeline.eject(player);
pipeline.handlerName();
```

The duplex handler is added immediately after the vanilla `decoder`, so it sees fully formed packet
objects in both directions. Anchoring is by handler **name**, not index, which is what survives
another plugin adding handlers around yours.

Every mutation is idempotent. A re-inject that finds the handler present is a no-op; an eject that
finds it gone is silent. So join, quit and reorder choreography can call these freely without
double-add crashes.

A player whose channel cannot be resolved, such as a mock under test, yields `false` rather than
throwing.

<Callout type="note" title="The pipeline schedules nothing">

Inject-on-join and delayed-reorder choreography belong to you, through the library `Scheduler`. That
keeps this class a pure pipeline mutator.

</Callout>

## The reorder watchdog

Another plugin can splice its handler ahead of yours after you injected, which silently changes what
you see.

```java
PipelineWatchdog watchdog = new PipelineWatchdog("myplugin-handler", PacketPipeline.DEFAULT_ANCHOR);

PipelineWatchdog.Decision decision = pipeline.reorder(player);
decision.needsReorder();
```

`evaluate(names)` is the pure form: hand it the current handler names and it tells you whether your
handler is still where it should be. Being pure is what makes it testable without a live connection.

## Resolving a channel

```java
ChannelResolver resolver = new ChannelResolver();
ChannelResolver anchored = new ChannelResolver("decoder");

Optional<Channel> channel = resolver.resolve(player);
```

## Listeners

```java
PacketListenerRegistry registry = new PacketListenerRegistry();

registry.register(listener);
registry.unregister(listener);
registry.isEmpty();
registry.snapshot();
```

A listener inspects a raw packet object and decides what happens to it:

```java
PacketListener listener = (player, packet) ->
        isBlocked(packet) ? PacketAction.CANCEL : PacketAction.PASS;
```

Override `onSendVerdict` to **replace** an outbound packet:

```java
PacketListener rewriter = new PacketListener() {
    @Override
    public PacketVerdict onSendVerdict(UUID player, Object packet) {
        return isTarget(packet) ? PacketVerdict.rewrite(modify(packet)) : PacketVerdict.pass();
    }
};
```

The default methods return `PASS`, so a listener implements only the direction it cares about, and a
listener written against `onSend` and `onReceive` alone keeps working unchanged.

<Callout type="warning" title="Callbacks run on a Netty I/O thread">

Never the main server thread. Do not touch the Bukkit API there; hand work to a `Scheduler`. A
listener must be cheap and must not block.

</Callout>

A callback that throws is swallowed and the packet passes. Fail-open, so a faulty listener can never
break a player's connection.

Packets cross this seam as an opaque `Object`. There is no typed wrapper layer yet.
