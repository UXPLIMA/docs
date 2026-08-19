---
title: Menu types
order: 231
description: Simple, paginated, scrolling, storage and typed menus, and the surface they all share.
icon: layout-panel-top
---

Five builders on `Guis`, one shared contract.

| Builder | Shape | Use it for |
|---|---|---|
| `Guis.gui()` | A fixed chest menu | Buttons and fixed layouts |
| `Guis.paginated()` | Pages of content slots | A list whose length you do not know |
| `Guis.scrolling(ScrollType)` | A window that slides one row or column | A grid that should feel continuous |
| `Guis.storage()` | Real items, take and place allowed, contents kept | Vaults, stashes, drop-here inputs |
| `Guis.typed(GuiType)` | Non-chest inventories | Hopper, dispenser, brewing stand and the rest |

## The shared surface

Every menu implements `Gui`.

### Placing items

```java
menu.set(13, item);            // raw slot
menu.set(2, 5, item);          // 1-indexed row and column
menu.addItem(a, b, c);         // into the next free slots
menu.getItem(13);              // the GuiItem at a slot, or null
menu.remove(13);
menu.clear();
menu.filler();                 // see Layout and animation
```

### Opening and closing

```java
menu.open(player);
menu.close(player);
menu.closeAll();
menu.preventClose(true);       // the viewer cannot close it themselves
menu.preventsClose();
```

### Handlers

```java
menu.onOpen(event -> ...);
menu.onClose(event -> ...);
menu.onDefaultClick(event -> ...);   // a click on a slot with no item
menu.onOutsideClick(event -> ...);   // a click outside the window
```

### Everything else

```java
menu.title();
menu.size();                   // rows times nine
menu.updateTitle(Text.mini("<red>New title"));
menu.refresh();                // re-resolve and rewrite every slot
menu.ticks();                  // the menu's own tick counter, driving animation
menu.getInventory();           // the backing Bukkit inventory
```

<Callout type="note" title="updateTitle reopens the inventory">

Bukkit fixes an inventory title at creation, so changing it means building a new inventory and
reopening it. The menu does that behind a reopen guard, so your `onClose` and `onOpen` handlers do
not fire and the open sound does not replay. The viewer sees a title change, not a menu flicker.

</Callout>

## Who can share a menu instance

This is the single most common mistake with any menu framework, so the module states it plainly.

A menu holding only **static** items can be opened for any number of players at once. They share one
inventory and all see the same thing, which is correct and cheap.

A menu holding **dynamic**, **stateful** or **animated** items is **single-viewer**. It resolves each
item for one viewer, so open a fresh instance per player. Sharing one such instance shows everybody
the first viewer's render.

`GuiNavigator` already opens a fresh instance per player, so navigation-driven menus get this right
without you thinking about it.

## SimpleGui

The default. A fixed chest of one to six rows.

```java
SimpleGui menu = Guis.gui().title(Text.mini("<dark_aqua>Menu")).rows(3).build();
```

## PaginatedGui

The full item list is the source of truth; each render projects a window of it into the content
slots. Add and clear items freely without recomputing slot maths.

```java
PaginatedGui shop = Guis.paginated().title(Text.mini("Shop")).rows(6).build();

products.forEach(p -> shop.addPageItem(GuiItem.button(p.icon(), e -> buy(p))));

shop.set(6, 4, GuiItem.previousPage(shop, leftArrow));
shop.set(6, 6, GuiItem.nextPage(shop, rightArrow));

shop.open(player);        // page 1
shop.open(player, 3);     // straight to page 3
```

| Method | Effect |
|---|---|
| `addPageItem(GuiItem)` | Append to the list |
| `populate(List<T>, ItemPopulator<T>)` | Replace the list from a domain collection |
| `clearPageItems()` | Empty the list, keeping decorations |
| `page()` / `pageCount()` | Where you are, how many there are |
| `nextPage()` / `previousPage()` | Turn, clamped at the ends |
| `render()` | Re-project the current page |

By default the whole menu is content. Narrow it with `contentSlots`:

```java
Guis.paginated().rows(6).contentSlots(List.of(10, 11, 12, 13, 14, 15, 16)).build();
```

<Callout type="tip" title="A set() inside the content region pins a decoration">

Calling `set` on a content slot pins a fixed icon there and the page projection pages around it. So
a separator or an arrow dropped into the content region wins over list content, and you do not have
to carve it out of `contentSlots` first. Only direct calls count; the page render's own writes never
register as decoration.

</Callout>

### populate

```java
shop.populate(products, (product, index) -> GuiItem.button(product.icon(), e -> buy(product)));
```

`ItemPopulator<T>` turns a domain object plus its index into a `GuiItem`, so the mapping from data to
icon lives in one lambda rather than a loop that also does slot arithmetic.

## ScrollingGui

Where paginated jumps a whole page, scrolling slides the window one row or one column.

```java
ScrollingGui warps = Guis.scrolling(ScrollType.VERTICAL).rows(4).build();
warps.addScrollItem(GuiItem.button(icon, e -> teleport()));

warps.set(4, 9, GuiItem.scrollNext(warps, downArrow));
warps.set(4, 1, GuiItem.scrollPrevious(warps, upArrow));
```

`VERTICAL` lays items out in reading order and reveals later rows. `HORIZONTAL` fills columns top to
bottom and reveals later columns.

`offset()` and `maxOffset()` tell you where the window sits, which is what you need to grey out an
arrow at either end.

## StorageGui

Holds real items rather than buttons. It allows take, place and swap from construction, and it does
not clear what the player leaves behind, so contents persist across opens.

```java
StorageGui vault = Guis.storage().rows(3).build();
vault.setContents(loadVault(player));
vault.onClose(e -> saveVault(player, vault.contents()));

Map<Integer, ItemStack> leftover = vault.addItem(reward);   // what did not fit, by index
```

`contents()` returns a snapshot array with `null` in empty slots.

## Typed menus

Non-chest inventories, each with a fixed size dictated by its type.

```java
SimpleGui hopper = Guis.typed(GuiType.HOPPER).title(Text.mini("Sort")).build();
```

| Type | Shape |
|---|---|
| `HOPPER` | 5 slots |
| `DISPENSER`, `DROPPER` | 9 slots, 3 by 3 |
| `WORKBENCH` | Result plus a 3 by 3 grid |
| `BREWING` | A brewing stand |
| `GRINDSTONE` | Two inputs and a result |
| `STONECUTTER` | Input and result |
| `CARTOGRAPHY` | Map, paper and result |
| `SMITHING` | Template, base, addition and result |
| `LOOM` | Banner, dye, pattern and result |
| `FURNACE` | Smelt, fuel and result, as a display surface |
| `BEACON` | One payment slot |
| `ENCHANTING` | Item and lapis |

`GuiType.size()` and `GuiType.inventoryType()` expose the backing values.

<Callout type="note" title="Recipe processing is neutralised, not disabled">

A grindstone or smithing table opened as a menu still has its vanilla result logic behind it. The
default cancel policy is what stops that logic ever producing an item the player can take. Do not
allow `ITEM_TAKE` on a typed menu unless you have thought about what its vanilla behaviour would
then do.

</Callout>
