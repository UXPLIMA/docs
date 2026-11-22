---
title: Menus
order: 185
description: The menu engine for modules: menu files, actions, list menus, hub buttons, dialogs and Bedrock forms.
icon: layout-grid
---

A menu is a YAML file; what a button does is an action your module registers in code. The same file is
drawn as a chest for Java players, as a Java dialog when the [Dialogs](../modules/dialogs.md) module is on
and the file allows it, and as a form for Bedrock players with the [Bedrock](../modules/bedrock.md) module.
You write the menu once, through `context.menus()` (package `com.uxplima.uxmclaims.sdk.menu`).

## Names

| Name | Means |
|---|---|
| `list` | Your module's menu or action `list`; unqualified ids are always your own |
| `example-shops:list` | The same, written in full |
| `lands:land-main` | Another module's menu, for example to add a hub button to it |
| `kernel:open-menu`, `kernel:previous-page`, `kernel:next-page`, `kernel:close` | The kernel's own actions, usable in any file. `kernel:open-menu` takes the `menu` argument |

## Where the files live

Ship each menu in your jar at `uxm/modules/<id>/lang/en/menus/<name>.yml` (and other language codes). The
server copy is `plugins/uxmClaims/modules/<id>/lang/<code>/menus/<name>.yml`. uxmClaims reads the server copy
first, then the jar copy in the viewer's language, then the English jar copy, then
`uxm/modules/<id>/menus/<name>.yml`. Menu files are reloaded with `/uxm reload`.

## Code

```java
import com.uxplima.uxmclaims.sdk.menu.HubButton;
import com.uxplima.uxmclaims.sdk.menu.MenuEntry;
import com.uxplima.uxmclaims.sdk.menu.MenuRegistry;
import com.uxplima.uxmclaims.sdk.module.ModuleContext;

import java.util.List;
import java.util.Map;

final class ShopMenus {

    static void register(ModuleContext ctx) {
        MenuRegistry menus = ctx.menus();
        menus.action("buy", (viewer, arguments) ->
                viewer.sendMessage("Buying " + arguments.get("item") + " with a " + arguments.get("click") + " click"));
        menus.entries("list", (viewer, arguments) -> List.of(
                MenuEntry.of(Map.of("item.name", "Sword", "item.price", "10"), "buy", Map.of("item", "sword")),
                MenuEntry.of(Map.of("item.name", "Bow", "item.price", "15"), "buy", Map.of("item", "bow"))));
        menus.action("open-list", (viewer, arguments) -> menus.open(viewer, "list", arguments));
        menus.hubButton("lands:land-main", new HubButton("shops", "open-list", 50));
    }
}
```

| Method | What it does |
|---|---|
| `action(id, MenuAction)` | What a button with `action: <id>` runs. It gets the viewer and the button's arguments, plus `click` (`PRIMARY`, `SECONDARY`, `SHIFT_PRIMARY` or `SHIFT_SECONDARY`) |
| `entries(menu, MenuEntrySource)` | The rows of a list menu, drawn with the file's `entry` template into its `entry-slots`, page by page. Called on the viewer's thread on every open: read memory only |
| `hubButton(menu, HubButton)` | Adds your button to another module's menu, in that menu's free `auto-slots`, ordered by `order`. A third argument, a `BiPredicate<Player, Map<String, String>>`, shows it only to some viewers |
| `open(viewer, name, arguments)` | Opens a menu; the arguments fill `%key%` placeholders and are passed to actions |
| `input()` | A `TextInput` that asks the player for one line of text through a Bedrock form, a Java dialog or chat |
| `confirm(player, TextPrompt, onConfirm, onCancel)` | A yes or no screen for players a confirm provider serves (Bedrock); returns `false` when none does, so keep your chat confirmation too |

A list row (`MenuEntry`) carries its placeholders, the action id a click runs (or `null` for the template's
own action) and its arguments, which are added on top of the menu's. A hub button's action gets the host
menu's arguments, so a button on `lands:land-main` receives that menu's `land`. Everything you register is
removed when your module stops; a button whose action is gone is hidden, or shows the filler with
`when-missing: filler`.

## The menu file

```yaml
config-version: 1
# example-shops:list. Arguments: land.
title: '<dark_gray>Shop</dark_gray>'
type: chest
rows: 3
entry-slots: [10-16]
entry:
  material: emerald
  name: '<green>%item.name%</green>'
  lore:
    - '<gray>Price:</gray> <white>%item.price%</white>'
items:
  back:
    action: kernel:open-menu
    arguments:
      menu: lands:land-main
      land: '%land%'
    slot: 22
    material: barrier
    name: '<red>Back</red>'
filler:
  material: black_stained_glass_pane
  name: ' '
```

| Key | What it does |
|---|---|
| `title` | MiniMessage title; `%key%` placeholders come from the menu's arguments |
| `type` | `chest` (default), `hopper`, `dispenser`, `dropper`, `workbench` or `brewing` |
| `rows` | Chest rows, 1 to 6 |
| `items.<key>` | A button: `slot` or `slots` (numbers and ranges, `[0, 10-16]`), `material`, `name`, `lore`, `amount`, `glow`, `model`, `texture`, `action`, `arguments` |
| `items.<key>.visible-if` | `permission:<node>`, `module:<id>` or `is:%placeholder%`, with an optional leading `!` |
| `items.<key>.when-missing` | `hide` (default) or `filler`, when the action's module is off |
| `items.<key>.player-commands`, `console-commands` | Commands run on click, as the viewer or the console, without the slash |
| `entry`, `entry-slots` | The row template and the slots rows fill, for menus with an entry source |
| `auto-slots` | Free slots for other modules' hub buttons |
| `decoration`, `filler` | Items without actions, and the item for every empty slot |
| `dialog` | `enabled` (default `false`), `columns`, `button-width`, `body`, `exit`: whether and how the menu shows as a Java dialog |

Message placeholders are written `<key>`; menu files use `%key%`. The look of your hub buttons goes in your
module's `menus/buttons.yml` under `buttons.<button id>`.

Bedrock players see the same buttons in slot order, decoration and filler left out. A file
`menus/bedrock/<name>.yml` next to the menu changes only what should look different on Bedrock: `enabled`,
`title`, `content`, `back`, and per item `label`, `image`, `hidden` and `clicks`. Server owners edit these
files as described in [Menus](../configuration/menus.md).

## Other renderers

A module can add another way to show menus: `renderer(UiRenderer)` draws a `UiScreen` (the page the chest
would show) and reports presses through `UiCallbacks`, `inputProvider(TextInputProvider)` asks for text, and
`confirmProvider(ConfirmProvider)` asks yes or no. Each says per player whether it `supports` them; the
highest `priority` wins, and the chest (or chat) is the fallback when none does or the chosen one throws.
The dialogs and bedrock modules are built on these.

Related: [Writing a module](sdk.md), [Commands](commands.md), [Menus configuration](../configuration/menus.md)
