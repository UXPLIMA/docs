---
title: Overview
order: 901
description: Every plugin and service uxmHeads works with.
icon: plug
---

| Plugin or service | Adds | Without it |
|---|---|---|
| PlaceholderAPI | Faces in every other plugin's lines | Only this plugin's own lines draw faces; the doctor warns |
| A skin service of your own | Faces from your skins | Add its host to `skins.hosts` |
| The account service | Skins by name for players without an account | Turn `find-by-name` off: those players wear the fallback |
| Geyser or Floodgate | The Bedrock face | Bedrock players get the Java rows through the bridge |
| uxmLang | One language choice for every UXM plugin | The choice is kept in `languages.conf` |

## Examples

**Skins from your own skin service.** Add its host to `skins.hosts`.

**No faces for Bedrock players.** Set `bedrock.face = "hidden"`.

Related: [Faces and skins](../features/faces-and-skins.md)
