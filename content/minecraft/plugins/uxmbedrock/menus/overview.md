---
title: Overview
order: 401
description: The forms of uxmBedrock and the forms it makes from other plugins.
icon: layout-panel-top
---

uxmBedrock opens no chest windows of its own: every screen it shows is a Bedrock form, written in the reader's
language from the message files.

| Form | Opens |
|---|---|
| [Settings form](settings.md) | `/bedrock` |
| [Animated pack question](animated.md) | Once, at a player's first join |
| A menu as a form | When another plugin opens a chest menu; see [Menus as forms](../features/forms.md) |
| Sign form | When another plugin opens a sign editor |
| Anvil form | When another plugin opens an anvil for typing |

| Form | Message lines |
|---|---|
| Settings | `settings.title`, `settings.intro`, `settings.menus`, `settings.menus-form`, `settings.menus-chest`, `settings.animated`, `settings.animated-full`, `settings.animated-light`, `settings.animated-none`, `settings.features.<feature>`, `settings.saved` |
| Animated question | `animated.title`, `animated.question`, `animated.full`, `animated.light`, `animated.none` |
| Sign | `forms.sign-title`, `forms.sign-line` |
| Anvil | `forms.anvil-label` |
