---
title: Menus
order: 108
description: Built-in Menu Guide, Custom Menu Engine, Actions & Requirements, Bedrock
  Forms, Converting Other Menus and In-Game Editor.
---

- [Built-in Menu Guide](built-in.md): Almost every GUI you open in uxmEssentials: the home grid, the warp browser, the kit list, the vault selector, the /eco and /mod admin panels, the module hub behind /uxmess gui: is drawn by one shared menu engine. There is no separate "GUI system" bolted onto each feature: the same code that loads your own menus/.conf files loads and renders the built-in menus too.
- [Custom Menu Engine](engine.md): A custom menu is a single HOCON file in plugins/uxmEssentials/menus/. The file name (without .conf) is the menu's name: menus/shop.conf is opened with /menu open shop. uxmEssentials ships menus/example.conf as a starting point: copy it, rename it, and edit.
- [Actions & Requirements](actions-requirements.md): Everything a menu does is an action; everything it checks is a requirement (also called a condition). Both are written the same way (a bare id:value reference) and both are shared by the built-in menus and your own.
- [Bedrock Forms](bedrock.md): Bedrock players (those joining a Java server through Geyser with Floodgate installed) do not see chest GUIs the way Java players do. A chest menu is clunky on a touch screen. So when uxmEssentials detects a Bedrock viewer, it renders the very same menu as a native Bedrock form instead: the clean, scrollable, tappable UI Bedrock players already know.
- [Converting Other Menus](converters.md)
- [In-Game Editor](editor.md)
