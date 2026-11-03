---
title: Other plugins
order: 302
description: How every UXM plugin reads the choice, what happens to their own language commands, and plugins without a file.
icon: puzzle
---

uxmLang registers the language service of uxmLib. Every UXM plugin built on uxmLib asks it which language a player
reads, so one choice covers them all: their commands, windows and every line they draw for a player.

| With uxmLang | Without uxmLang |
|---|---|
| One choice, in uxmLang's database, shared by every UXM plugin | Each plugin keeps its own choice in its own `languages.conf` |
| A plugin's own `/<plugin> lang set` writes the same choice | Each plugin's command writes its own |
| On a network sharing the database, one choice for every server | One choice per plugin per server |

A plugin draws a line in the chosen language when it carries a file for it. Each UXM plugin ships the same seven
languages; a language you add to uxmLang but not to a plugin reads in that plugin's `language.default`.

Related: [Concepts](../getting-started/concepts.md#one-choice-for-every-plugin)
