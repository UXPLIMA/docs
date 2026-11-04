---
title: Commands for Bedrock
order: 1308
description: Argument completion and finding Bedrock players by name.
icon: terminal-square
---

| Switch | What changes |
|---|---|
| `command-completion` | Bedrock asks nobody for completions while a player types, so a Bedrock player sees only the words the command list holds. The first argument values a Java player would be offered (homes, warps, kits, names) are added to the list for each command, and refreshed after each command they run |
| `name-finder` | A command that names a player who is not online is tried again with Floodgate's name prefix, so `/msg Steve` reaches `.Steve`. Only the first three arguments are looked at |

On a network, uxmBedrock-Velocity does the same completion for the proxy's own commands; see
[A network](network.md).
