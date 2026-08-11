---
title: In-Game Editor
order: 1130
---

## Edit menus in-game: no config files

The **in-game menu editor** lets you build and change custom menus from inside the
game, without ever opening a `.conf` file by hand. Run **`/menu editor`** and you
get a browser of every custom menu on your server, with buttons to create, copy,
rename and delete them, a visual slot grid to arrange items, full item editing,
click-action & requirement builders, menu-wide settings, quick item capture and a
live preview: all with validated save and hot-reload.

<Callout type="success" title="Complete">

Everything the menu engine can express is editable in-game: the **menu browser,
the visual slot grid, full item editing, the click-action & requirement builders,
menu-wide settings** (title, size, open-command and more), **item capture from
your hand, an edit lock** so two people can't clobber the same menu, and a **live
preview**. You never need to touch a `.conf` file.

</Callout>

---

## Opening the editor

**`/menu editor`** opens the menu browser: one entry per custom menu (the files in
`plugins/uxmEssentials/menus/`), showing each menu's title, size and item count.

From the browser you can:

| Action | What it does |
|--------|--------------|
| **Create** | Type a name and a fresh, empty menu is created and saved. |
| **Duplicate** | Copy an existing menu to a new name. |
| **Rename** | Give a menu a new name (its file is renamed too). |
| **Delete** | Remove a menu, with a confirmation step so nothing goes by accident. |
| Click a menu | Open its overview to save it (and, soon, edit its contents). |

It's also reachable from the management hub: `/uxmess gui`.

<Callout type="tip" title="Every change is validated and hot-reloaded">

When you save, the editor checks that every action and requirement the menu
references actually exists, then writes the file and reloads that one menu live:
no server restart, and a menu with a typo is caught before it's written.

</Callout>

---

## The slot grid

Open a menu from the browser and you get a **live grid that mirrors the real
menu**: every item drawn in its actual slot, empty slots shown as clickable
placeholders, and a control bar along the bottom (Back, Save, and page arrows for
bigger menus). Your **own inventory stays usable underneath it**, so the fastest way
to build a menu is to drag items straight from your bags onto the grid.

| Do this | To… |
|---------|-----|
| **Drag an item from your inventory onto a slot** | Drop a copy of it there, exactly as it is: name, lore, enchants, custom model and all. Your real item stays in your bag. |
| **Pick an item onto your cursor, then click slots** | "Stamp" a copy into every slot you click: fill a whole row of the same item in seconds. |
| **Shift-click an item in your inventory** | Send a copy to the next empty slot: tap through a stack of items to lay them out fast. |
| Click an empty slot with an **empty cursor** | Add a plain item there and open its editor. |
| Click a **filled** slot | Open its item editor (name, lore, actions…). |
| Right-click an item, then click another slot | Pick it up and move it (swap if the target is filled). |
| Shift-click an item **in the grid** | Clear that slot (with a confirmation). |
| **Preview** | Open the menu exactly as a player would see it, without saving. |
| Save | Write your changes and reload the menu live. |

<Callout type="tip" title="Dragging copies; it never eats your items">

The grid only ever **copies** the item's look into the menu; the real stack never
leaves your inventory. Drag the same diamond onto ten slots and you still have one
diamond. Use your survival or creative inventory as a palette without worrying.

</Callout>

You're always editing a **working copy**: the real menu only changes when you hit
Save, so a menu someone else has open won't shift under them mid-edit. And while
you have a menu open in the editor it's **locked**: anyone else who tries to edit
the same menu is told you're editing it, so two people can't save over each other.
The lock lifts when you leave the editor or log off.

---

## Editing an item

Click an item in the grid and you get a property panel: the same kind of editor
uxmEssentials uses for kits, warps and worlds, where each row is one setting:

| Setting | How you edit it |
|---------|-----------------|
| **Material** | Type any material or token (`DIAMOND`, `head:…`, `itemsadder:…`), or press **capture from hand** to grab whatever you're holding, exactly as it is. |
| **Name** | Type it in an anvil (supports MiniMessage colours). |
| **Lore** | Add, edit, remove and **reorder** lines one by one. |
| **Amount / Priority / Model-data** | Step up/down (hold shift for bigger jumps). |
| **Glow & item flags** | Toggle on/off. |
| **Lore mode / Item type** | Pick from a list. |
| **Slots** | Set which slot(s) the item occupies. |

Changes land on your working copy as you make them; **Back** returns to the grid,
and **Save** writes everything at once.

---

## Click actions & requirements

An item's editor also has **Click Actions** and **Requirements** rows, so you can
wire up behaviour without knowing the config syntax:

- **Click actions**: pick a click type (left, right, shift-left, …), then build a
  list of things that happen on that click. **Add** picks an action from a menu of
  everything the plugin supports (message, open another menu, run a command, give
  money, play a sound, and dozens more), then you fill in its value. You can
  **reorder** and remove them.
- **Requirements**: decide who sees or can use the item: add conditions (a
  permission, enough money, an item in hand, a placeholder comparison…) from the
  same kind of picker, and set how many must pass.

Because the action and condition lists come straight from the live plugin, you can
only pick things that actually exist, and every menu is re-checked before it saves,
so a broken reference is caught rather than written.

---

## Building from your inventory

The fastest way to fill a menu is to use your own inventory as a palette and **drag
items onto the grid**:

- **Drag** an item from your bags onto a slot: a copy lands there, exactly as it is
  (custom name, lore, enchants, model data, custom-model items, the lot), captured
  byte-for-byte. Your real item never moves.
- **Stamp**: pick an item onto your cursor, then click one slot after another to
  drop a copy into each. Great for filling a border or a row with the same item.
- **Shift-click** an item in your inventory to send a copy to the next empty slot:
  tap down a stack of items and they lay themselves out in order.
- **By command**: `/menu captureitem <slot>` puts whatever you're **holding in your
  hand** into that slot of the menu you're currently editing (handy when you've
  closed the grid window to type). An empty hand or an out-of-range slot just tells
  you and changes nothing.

Captured items round-trip perfectly, so a fully-built item from your inventory
becomes a menu button with zero fuss. To fine-tune afterwards, click the slot and
use the item editor as normal (its **capture from hand** button does the same thing
for the material field).

---

## Live preview

Hit **Preview** in the grid to open the menu **exactly as a player would see it**:
rendered through the real menu engine, with your unsaved changes, but without
committing anything to disk. It's a look-only view: close it to return to the
editor. Preview it, tweak it, preview again, and only **Save** when it's right.

---

## Menu-wide settings

Open a menu's overview (click it in the browser) to edit the whole menu, not just
its items:

| Setting | What it controls |
|---------|------------------|
| **Title** | The menu's name at the top. |
| **Rows** | How big the menu is, 1–6 (shrinking drops items that fall off the end). |
| **Inventory type** | Chest, hopper, dropper or dispenser. |
| **Click cooldown** | A small delay between clicks, to stop spam. |
| **Chest-only / Bottom-inventory** | Rendering options for how the menu behaves. |
| **Open requirement** | Conditions a player must meet to open the menu at all. |
| **Open / Close actions** | Things that happen when the menu opens or closes. |
| **Refresh** | Whether the menu re-draws itself on a timer, and how often. |
| **Open command** | Give the menu its own command (`/shop`), with aliases, a permission and a deny message. |

From here you also jump to the grid, **Save** the whole menu, or **Delete** it.

---

## Naming rules

Menu names use letters, numbers, `-` and `_`. A few names are reserved for the
engine's own files (`openers`, `patterns`, `placeholders`, `example`) and can't be
used, and you can't overwrite a menu that already exists by accident.

---

## Permissions

| Node | What it grants | Default |
|------|----------------|---------|
| `uxmessentials.menu.editor` | Use `/menu editor` and edit menus in-game | Operators |

---

## Next Steps

- 🎮 [Custom Menu Engine](engine.md): the menu spec the editor reads and writes
- 🧩 [Actions & Requirements](actions-requirements.md): what you can wire onto a click
- 🔄 [Converting Other Menus](converters.md): import from DeluxeMenus/zMenu/OGUI/GUIPlus, then edit here
