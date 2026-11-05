---
title: vault.conf
order: 652
description: The shared island vault and its pages.
icon: archive
---

The island vault: a chest of several pages every member of the island shares, /is vault.

The file is `plugins/uxmSkyblock/modules/vault.conf`. Every default below is the shipped value.

## `vault`

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | false turns the island vault off. |
| `base-pages` | `1` | How many pages an island's vault has before the vault pages upgrade adds more. |
| `max-pages` | `10` | The most pages a vault can reach. |
| `slots-per-page` | `45` | How many slots a vault page holds, a multiple of 9 from 9 to 54. The page is drawn by menus/island-vault-page.conf, whose "skyblock:vault-page" region must hold as many slots; until it does, the vault opens in a plain chest of this size. A page that held more slots keeps the items past them and shows them again when this is raised. |
| `lease-duration-seconds` | `60` | One member edits a page at a time. How many seconds a page stays locked to the member who opened it before another member may take it over. |
| `audit-log-limit` | `50` | How many lines of who put in and took out what are kept for each page. |
| `expired-session-sweep-interval` | `5m` | How often the sessions whose lease ran out are closed and their page unlocked. A player who crashes mid edit leaves one behind, and nothing else closes it. |
