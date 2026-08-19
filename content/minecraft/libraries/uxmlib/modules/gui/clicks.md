---
title: Clicks and safety
order: 233
description: The default-deny cancel policy, interaction classes, click debounce, the anti-desync re-check, and declarative or asynchronous handlers.
icon: mouse-pointer-click
---

Everything on this page is about one question: what happens between a player clicking a slot and your
code running. A menu framework that gets this wrong duplicates items.

## Default deny

Every interaction class starts denied. An unconfigured menu cancels every click and drag, so its
items cannot be taken, replaced, dragged out or dropped. You opt in per class.

```java
menu.allow(InteractionModifier.ITEM_TAKE);
menu.disallow(InteractionModifier.ITEM_TAKE);
menu.allows(InteractionModifier.ITEM_TAKE);
```

Or at build time:

```java
Guis.gui().rows(3).allow(InteractionModifier.ITEM_TAKE, InteractionModifier.ITEM_PLACE).build();
```

## Interaction classes

| Modifier | Covers |
|---|---|
| `ITEM_TAKE` | Taking an item out of a menu slot |
| `ITEM_PLACE` | Putting an item into a menu slot |
| `ITEM_SWAP` | Swapping the cursor, or a hotbar slot, with a menu slot |
| `ITEM_DROP` | Dropping an item with Q while the menu is open |

A single click can need more than one. A hotbar number-key swap is both a take and a swap, and both
must be allowed for it to pass. A shift-move is a take when it pulls out of the menu and a place when
it pushes into it, decided by direction, not by the key pressed.

Actions that only touch the player's own inventory need nothing, and neither do `NOTHING`,
`CLONE_STACK` or `UNKNOWN`.

`StorageGui` allows take, place and swap from construction. Everything else starts at zero.

## Click debounce

Clicks arriving within about 150 milliseconds of a viewer's last accepted click are dropped. This is
what kills the shift-spam duplication and the rapid double-fire that desyncs a menu when a handler
opens another inventory.

The debounce table is per `GuiListener`, so two plugins each with their own `Guis.install` do not
share a cooldown. It is keyed by viewer and pruned on menu close and on quit, so it stays bounded to
online players.

## The anti-desync re-check

A dynamic, stateful or animated icon can change between the render a player is looking at and the
moment they click it. Before a declarative action runs, the framework confirms the slot still holds
the icon the click targeted. If it no longer matches, the action is skipped and the cancel stays in
place.

This is the difference between "the button did what it showed" and "the button did what it showed a
tick ago". It costs one item comparison because icons already resolve from a `RenderContext`.

<Callout type="warning" title="The re-check guards declarative actions">

The re-check runs on the `GuiAction.Responding` path. A plain `GuiItem.button` handler runs against
the live event as Bukkit delivered it. If your button's meaning changes per render, prefer
`GuiItem.responding`.

</Callout>

## Declarative click handlers

A declarative handler is a pure function: it takes an immutable `ClickContext` and returns a list of
`GuiResponse` values describing what should happen. The framework applies them, in order, on the
viewer's region thread.

```java
GuiItem.responding(icon, ctx -> {
    if (!canAfford(ctx.viewer())) {
        return List.of(GuiResponse.playSound(denySound));
    }
    charge(ctx.viewer());
    return List.of(
            GuiResponse.playSound(buySound),
            GuiResponse.updateItem(ctx.slot(), soldOutIcon),
            GuiResponse.close());
});
```

`ClickContext` is a record of `viewer`, `slot`, `clickType`, `clickedItem` and `cursor`. It is a
snapshot, not the live event, so it stays valid even when the responses land a tick later.

### The response set

| Response | Effect |
|---|---|
| `GuiResponse.close()` | Close the menu for the viewer |
| `GuiResponse.open(gui)` | Open another menu instead |
| `GuiResponse.refresh()` | Re-resolve and rewrite the current menu |
| `GuiResponse.updateItem(slot, item)` | Place an item at a slot |
| `GuiResponse.replaceCursor(stack)` | Replace what is on the viewer's cursor |
| `GuiResponse.playSound(sound)` | Play a sound to the viewer |
| `GuiResponse.run(task)` | Run arbitrary code on the viewer's region thread |
| `GuiResponse.nothing()` | Explicitly do nothing |

The point of modelling effects as data is that the handler becomes unit-testable without a live
inventory, and safe to compute off the main thread.

## Asynchronous clicks

A click that needs a database round trip should not block the server thread while it happens.

```java
GuiItem.respondingAsyncButton(icon, ctx ->
        profiles.loadAsync(ctx.viewer().getUniqueId())
                .thenApply(profile -> List.of(GuiResponse.open(profileMenu(profile)))));
```

The heavy work runs wherever your future puts it. Only the response **application** goes through the
library `Scheduler`, and only when the future is not already complete: a handler that finishes
synchronously takes a fast path with no scheduler hop at all.

A handler that throws routes its cause through the error path, so the failure is logged once and
never swallowed.

<Callout type="note" title="Async clicks need the Scheduler overload">

`Guis.install(plugin, scheduler)` is what gives the framework a thread to marshal responses back
onto. Without it, responses are applied inline.

</Callout>

## The click log

A bounded in-memory record of recent clicks, for debugging a misbehaving menu without attaching a
debugger.

```java
GuiClickLog log = Guis.clickLog();
log.recent().forEach(entry -> logger.info(entry.toString()));
```

Each entry carries the instant, the viewer, the menu title, the slot and the click type. It keeps the
last 100 clicks by default, oldest evicted first, and it is synchronized so an admin command can read
it while a Folia worker writes it.
