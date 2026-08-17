---
title: Installation
order: 12
description: Requirements, first run, and the settings to fix before opening the server.
icon: download
---

## Requirements

| Requirement | Detail |
|---|---|
| Server | 1.13 or newer (`api-version: 1.13`) |
| Vault | Required — selling goes through it |
| NBTAPI | Required — spawner items are identified by NBT |
| Load order | The plugin loads `POSTWORLD`, after worlds exist |

Everything else is optional and hooked only if present.

## Install

1. Drop the jar into `plugins/` along with Vault and NBTAPI, then start the server.
2. The plugin writes `config.yml`, a `modules/` folder, a `lang/` folder and `loottables/`.
3. Set `config.lang` to one of `en`, `tr`, `pl`, `de`, `fr`.
4. Fill in `license-key`.
5. Work through `modules/` — each file is one feature and can be turned off on its own.

<Callout type="danger" title="RoseStacker needs one setting changed">

If RoseStacker is installed you **must** enable `trigger-death-event-for-entire-stack-kill` in
RoseStacker's config. Without it, stacked entities are not counted correctly and spawner storage
silently loses drops.

</Callout>

## First checks

| Option | Where | Why it matters on day one |
|---|---|---|
| `disabled-worlds` | `config.yml` | Nobody can place or break any spawner there, vanilla included |
| `use-modern-autokill` | `config.yml` | On 1.16+ leave it on: loot is generated directly instead of spawning and killing mobs |
| `auto-save.interval` | `config.yml` | 300 seconds is the recommended default; this is what survives a crash |
| `vanilla-spawner-support` | `modules/other.yml` | Decides what happens when someone breaks a spawner that was already in the world |
