---
title: Centring and colours
order: 302
description: Centring by font pixels, colour roles, painting a name across the wheel, small capitals and plain words.
icon: type
---

## Centring

Start a line with `<center>` and the plugin measures it in font pixels and pushes it to the middle of the box the client
draws in. Centring by counting characters looks wrong, because the font is not fixed width.

`centring.box-width-pixels` (267) is the width of that box: what the vanilla client wraps the description at, measured
from the client itself. Lower it if your players use a resource pack with a wider font.

## Roles

Name a role instead of a colour:

| Role | For |
|---|---|
| `<value>` | A number or a version |
| `<good>`, `<warn>`, `<bad>` | A state |
| `<info>` | A note |
| `<rank>` | Something a player earns |
| `<accent>` | The brand |
| `<event>` | An event or a store |
| `<icon>` | A bullet or an arrow |
| `<body>` | Ordinary text |
| `<subtext>` | A quieter second line |

The colours behind those names are in [theme.conf](../config/theme.md), so one edit there repaints every line.

## The network's name

`<g:'NAME':wheel>` asks the theme for every colour of its wheel, in order, and paints the name across them, so the
colours stay in the theme and `config.conf` names none of them. `<g:'NAME':0>` paints across one arc of the same wheel,
as the shipped maintenance entry does.

## Small capitals

English is written in small capitals, as the rest of the plugin is: write ordinary words and the plugin converts them.
Wrap a word in `<plain>` to keep it in ordinary letters, which an address needs: `<plain>play.uxm.net</plain>`.

Related: [Entries](entries.md), [theme.conf](../config/theme.md)
