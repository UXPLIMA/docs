---
title: Admin commands
order: 306
description: Every /claimadmin branch, its node and what it does.
icon: shield
---

Each node is `uxmsimpleclaims.admin.<node>`.

| Command | Turkish | Node | Does |
|---|---|---|---|
| `/claimadmin` | | `version` | The usage line |
| `/claimadmin reload` | `yenile` | `reload` | Reads the configuration again |
| `/claimadmin version` | `surum` | `version` | The running version |
| `/claimadmin doctor` | `durum` | `doctor` | Storage, windows, claims and placeholders |
| `/claimadmin bypass` | `gecis` | `bypass` | Ignores every claim, or stops ignoring them |
| `/claimadmin stick` | `cubuk` | `inspect` | The stick that says who owns a block |
| `/claimadmin inspect` | `incele` | `inspect` | Says who owns the chunk you stand in |
| `/claimadmin limits <player>` | `limitler` | `limits` | What a player may have |
| `/claimadmin delete` | `sil` | `delete` | Deletes the claim you stand in, whoever owns it |
| `/claimadmin transfer <player>` | `devret` | `transfer` | Gives the claim you stand in to a player |
| `/claimadmin resetflags` | `ayarsifirla` | `flags` | Puts the flags of the claim here back |
| `/claimadmin resetflags all` | `hepsi` | `flags` | Puts the flags of every claim back |
| `/claimadmin snapshots [player]` | `yedekler` | `snapshots` | Lists the deleted claims |
| `/claimadmin restore <snapshot>` | `geriyukle` | `snapshots` | Puts a deleted claim back |
| `/claimadmin profile <player>` | `profil` | `profile` | Opens a player's claims |
| `/claimadmin audit` | `gecmis` | `audit` | Reads the latest changes to the claim you stand in |

`bypass`, `stick`, `inspect`, `delete`, `transfer`, `resetflags`, `profile` and `audit` are for players only.

Related: [Staff tools](../features/staff-tools.md)
