---
title: Layout and animation
order: 234
description: Fillers, mask patterns, adaptive slot layouts, and moving-highlight slot animations.
icon: grid-3x3
---

## The filler

`menu.filler()` returns a fluent `GuiFiller` over the menu. Every method returns the filler, so calls
chain.

```java
menu.filler()
        .fillBorder(GuiItem.display(pane))
        .fillRow(3, GuiItem.display(divider))
        .fillEmpty(GuiItem.display(background));
```

| Method | Fills |
|---|---|
| `fill(GuiItem)` | Every slot |
| `fill(List<GuiItem>)` | Every slot, cycling the list |
| `fillEmpty(GuiItem)` | Only the slots still free |
| `fillBorder(GuiItem)` | The outer ring |
| `fillBorder(int offset, GuiItem)` | A ring inset by `offset` rings |
| `fillRow(int row, GuiItem)` | One 1-indexed row |
| `fillColumn(int col, GuiItem)` | One 1-indexed column |
| `fillRect(r1, c1, r2, c2, GuiItem)` | A rectangle between two corners |
| `pattern(List<String>, Map<Character, GuiItem>)` | A character mask |

Order matters. `fillEmpty` after your buttons is background; `fill` after your buttons erases them.

## Mask patterns

The mask is the readable way to lay a menu out, and it is the same shape a config-defined menu uses.

```java
menu.filler().pattern(
        List.of(
                "XXXXXXXXX",
                "X  A B  X",
                "XXXXXXXXX"),
        Map.of(
                'X', GuiItem.display(pane),
                'A', GuiItem.button(acceptIcon, e -> accept()),
                'B', GuiItem.button(cancelIcon, e -> cancel())));
```

A space, or any character with no legend entry, leaves the slot alone.

`GuiFiller.orderedBorderSlots(width, height, offset)` is the static behind `fillBorder`, exposed so
you can compute a ring yourself.

## Layout arithmetic

`GuiLayout` is pure, side-effect-free slot maths, kept static so it is testable without an inventory.

| Method | Returns |
|---|---|
| `contentRows(rows)` | Rows left for content, reserving the bottom row for navigation once the menu has one to spare |
| `adaptivePageSize(rows)` | A sensible page size for a chest of that height, never below one |
| `clampPageSize(count)` | A caller-supplied page size clamped to at least one |
| `fillCapacity(GuiType)` | The true slot count of a non-chest menu, which is not `rows * 9` |
| `adaptiveSlots(count, layouts)` | The nicest fixed layout for that many items |
| `singleRowLayouts()` | A ready-made count-to-slots map for small pages |

```java
List<Integer> slots = GuiLayout.adaptiveSlots(rewards.size(), GuiLayout.singleRowLayouts());
```

One item centres, two sit either side of centre, three make a tidy row, five fill it. This is the
difference between a reward menu that looks designed and one that looks left-aligned.

## Slot animation

A moving highlight over the menu: a pattern of lit slots plus the icon shown in them, advanced on the
menu's tick clock.

```java
SlotPattern pattern = SlotPattern.clockwiseBorder(9, 3, 4, 0);
menu.addAnimation(SlotAnimation.of(pattern, ItemBuilder.of(Material.LIME_STAINED_GLASS_PANE).build()));
```

| Factory | Pattern |
|---|---|
| `SlotPattern.clockwiseBorder(width, height, litCount, offset)` | A run of lit slots chasing round the border |
| `SlotPattern.sweep(width, height)` | A column sweeping left to right |
| `SlotPattern.of(List<List<Integer>>)` | Your own frames, as slot lists |

Each advance diffs the new frame against the last and writes only the slots that changed, so a
border animation is a handful of item writes per tick rather than a full re-render.

<Callout type="note" title="An animation never paints over a button">

A slot is lit only when it is free or still shows this overlay's own highlight, and cleared only when
it still shows that highlight. Place a button onto a lit slot afterwards and the overlay loses
ownership of it: it will not re-paint over the button, and it will not later clear it.

</Callout>

Animation needs the `Scheduler` overload of `Guis.install`. Without it, `addAnimation` is accepted
and inert.

### Reading a pattern directly

`SlotPattern` is a value you can inspect and test.

```java
pattern.frameCount();
pattern.frame(2);
pattern.frameIndexAt(ticks);
pattern.slotsAt(ticks);
pattern.diff(fromFrame, toFrame);   // toClear and toLight
```

## Auto refresh

```java
Guis.gui().rows(3).autoRefresh(Duration.ofSeconds(1)).build();
```

Re-resolves every item on a timer while the menu is open. This is how a dynamic icon showing a live
balance or a countdown stays current without you scheduling anything. It also needs the `Scheduler`
overload.
