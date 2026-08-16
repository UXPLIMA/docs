---
title: Custom Menus
order: 9030
description: 'The operator surface over the menu engine: your own menus, behind /menu.'
---

Every GUI in uxmEssentials is drawn by one menu engine, and this module is the operator's door to it: your own
menus, written as HOCON files in `plugins/uxmEssentials/menus/` or built in game, opened with `/menu` and
editable with the in-game editor.

Module `custommenus` · enabled by default · `modules/custommenus/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/menu` | Open, list, reload, execute, dump, save, edit, capture into, and convert operator custom menus. | `uxmessentials.menu.use` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.menu.admin` | op | /menu reload to re-read the menus/ folder from disk. |
| `uxmessentials.menu.editor` | op | /menu editor (and the /uxmess gui hub entry) to create, duplicate, rename and delete custom menus in-game. |
| `uxmessentials.menu.open.others` | op | /menu open \<menu> \<player>: open a custom menu for somebody else. |
| `uxmessentials.menu.use` | everyone | /menu open \<name> to open an operator custom menu, and /menu list to see the loaded menus. |
| `uxmessentials.module.custommenus` | op | Hot-reload / inspect the custommenus module (operator custom menus behind /menu). |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `allow-console` | `false` | Whether a custom menu may run a command from the CONSOLE (the `console` menu action). This is privileged; a console command runs with full server permissions regardless of who clicked, so it is OFF by default. Leave it false unless you trust every menu file in the menus/ folder, and prefer the `command` action (which runs as the clicking player) wherever possible. Our own built-in feature menus are unaffected by this flag; it governs only operator menu files. When false, a menu that references the `console` action still loads, but that action does nothing and logs a warning naming the ignored command, so a misconfigured menu is visible in the console rather than silently privileged. |
| `click-cooldown-ms` | `0` | A server-wide anti-spam floor (in milliseconds) for menu clicks. When a player clicks a menu item, any further click on that menu inside this window is ignored, so a rapid double-click cannot fire a button's actions twice; handy for buy/sell or reward buttons. Zero (the default) disables the floor entirely, so menus behave exactly as before unless you opt in. An individual menu may raise this with its own `click-cooldown` key in the menu file; a menu never opts below the floor set here. The block is silent; no "slow down" message is sent, since that would itself spam the chat. |
| `default-currency` | `"vault"` | The currency a money menu action or requirement uses when it names none of its own. The menu engine can target several economy back-ends by a short spec: `vault` (the server economy), `exp` (native experience points), `playerpoints`, `coinsengine` / `coinsengine:<name>`, and `zessentials` / `zessentials:<name>` (the colon picks a named sub-currency; the bare name uses that plugin's default). A back-end whose plugin is absent simply does nothing. This is the fallback only; an individual menu action may still override it. Defaults to vault, the out-of-the-box server economy. |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_menu_argument_<name>%` | The value of one named argument the open menu was called with. |
| `%uxmessentials_menu_is_in_menu%` | Whether the player has a plugin menu open (yes/no). |
| `%uxmessentials_menu_last%` | The id of the last menu the player opened, which survives the menu closing. |
| `%uxmessentials_menu_opened%` | The id of the menu the player has open. |
| `%uxmessentials_menu_page%` | The page the open menu is showing, counting from 1. |
| `%uxmessentials_menu_rows%` | How many rows the open menu has. |
{/* /generated */}

## Notes

- **A bad menu never takes the others down.** A menu naming an action, condition or placeholder the engine does
  not know is skipped with a console warning rather than failing the load.
- **Console actions are privileged and ship off.** A console action runs with full server permissions whoever
  clicked it, so `allow-console-actions` has to be turned on deliberately.
- **A click window stops a double-click firing twice,** which matters most on a menu that pays out or charges.
- **The full grammar lives with the engine**, not here: this module only decides whether your own menus are
  loaded and what they may do.

Related: [Menus](../menus/engine.md), [Menu editor](../menus/editor.md), [Holograms](holograms.md)
