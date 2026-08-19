---
title: Items
order: 232
description: Static, dynamic, stateful and animated icons, the render context they resolve against, and display modifiers.
icon: box
---

A `GuiItem` is an icon to show plus an action to run on click. It is a sealed set of four kinds, all
resolved through a `RenderContext`, which is why one menu can look different for every viewer.

| Kind | Icon | Shareable across viewers |
|---|---|---|
| `Static` | Fixed | Yes |
| `Dynamic` | Computed per viewer at render time | No |
| `Stateful` | The first matching state, per viewer | No |
| `Animated` | Cycles frames on a timer | No |

## Factories

```java
GuiItem.display(stack);                          // no click
GuiItem.button(stack, event -> ...);             // fixed icon, handler

GuiItem.dynamic(ctx -> headOf(ctx.viewer()));    // per viewer, no click
GuiItem.dynamic(ctx -> icon(ctx), event -> ...); // per viewer, with a handler

GuiItem.stateful()
        .display(ctx -> ctx.viewer().hasPermission("vip"), vipIcon)
        .display(ctx -> true, normalIcon)
        .build();

GuiItem.animated(List.of(frame1, frame2), Duration.ofMillis(250));
GuiItem.animated(frames, interval, event -> ...);
```

`stateful` picks the **first** matching state, so order the predicates from most specific down to a
`true` fallback. Without a fallback, a viewer matching nothing sees an empty slot.

`state(...)` on the stateful builder is the fuller form when a state also needs its own click
behaviour; `display(...)` is the shorthand for a state that is display-only.

## Ready-made navigation buttons

```java
GuiItem.back(navigator, backArrow);
GuiItem.nextPage(paginatedGui, rightArrow);
GuiItem.previousPage(paginatedGui, leftArrow);
GuiItem.scrollNext(scrollingGui, downArrow);
GuiItem.scrollPrevious(scrollingGui, upArrow);
```

These exist so a back button does not need to know which menu it came from and a page arrow does not
need a closure over the menu you are still building.

## Declarative buttons

The handler receives an immutable snapshot and returns the effects to apply, instead of mutating the
menu itself.

```java
GuiItem.responding(icon, ctx -> List.of(
        GuiResponse.playSound(clickSound),
        GuiResponse.updateItem(ctx.slot(), newIcon),
        GuiResponse.close()));
```

```java
GuiItem.respondingAsyncButton(icon, ctx ->
        loadBalance(ctx.viewer().getUniqueId())
                .thenApply(balance -> List.of(GuiResponse.open(balanceMenu(balance)))));
```

The full model, and why it exists, is in [Clicks and safety](clicks.md).

## RenderContext

Every non-static icon resolves against one:

```java
public record RenderContext(Player viewer, Gui gui, int slot, Player effectivePlayer)
```

| Member | Meaning |
|---|---|
| `viewer()` | The player the menu is being rendered for |
| `gui()` | The menu itself |
| `slot()` | The slot being rendered |
| `effectivePlayer()` | Whose data the icon describes |
| `locale()` | The viewer's locale, for translated text |
| `withEffectivePlayer(Player)` | A copy pointing at a different subject |

`effectivePlayer` defaults to the viewer and matters when an admin opens somebody else's menu: the
viewer is the admin, the effective player is the target, and an icon that reads a balance should read
the target's.

## GuiAction

The sealed set behind a click:

| Kind | Behaviour |
|---|---|
| `GuiAction.None` | Nothing happens |
| `GuiAction.Run` | Runs a `Consumer<InventoryClickEvent>` |
| `GuiAction.Responding` | Returns a future of `GuiResponse`s the framework applies |

You rarely name these; the factories above produce the right one.

## Display modifiers

A `DisplayModifier` post-processes an icon after it is resolved and before it is shown, so a
cross-cutting concern is written once rather than in every icon.

```java
GuiItem head = DisplayModifiers.apply(
        GuiItem.display(ItemBuilder.of(Material.PLAYER_HEAD).build()),
        DisplayModifiers.of(
                DisplayModifiers.viewerSkull(),
                DisplayModifiers.placeholders((player, text) -> Placeholders.apply(player, text)),
                DisplayModifiers.loreSplit("|")));
```

| Modifier | Effect |
|---|---|
| `viewerSkull()` | Points a player head at the viewer |
| `placeholders(resolver)` | Runs the name and lore through a resolver, typically PlaceholderAPI |
| `loreSplit(token)` | Splits a single lore line on a token into several lines |
| `of(...)` | Composes several into one, applied in order |

`loreSplit` exists for config-defined menus, where a lore line arrives as one string an operator
wrote with a separator in it.
