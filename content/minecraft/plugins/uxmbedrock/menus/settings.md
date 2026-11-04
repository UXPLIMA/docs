---
title: Settings form
order: 402
description: What a Bedrock player chooses in /bedrock.
icon: sliders-horizontal
---

| Element | Kind | Does |
|---|---|---|
| Intro | Label | `settings.intro` |
| Menus | Dropdown: forms, chests | Other plugins' menus as forms or chests |
| Animated pack | Dropdown: full, light, none | How much animation the player downloads, from their next join |
| One toggle per feature | Toggle | Each feature in `player-choices.allowed` that the operator has on, labelled `settings.features.<feature>` |

Submitting saves every choice at once and says `settings.saved`. Closing the form changes nothing. A toggle for a
feature the operator turned off under `modules` is not shown.
