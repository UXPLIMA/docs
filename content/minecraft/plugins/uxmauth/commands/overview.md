---
title: Overview
order: 201
description: Every command at a glance, Turkish words, and who may run what.
icon: list
---

| Command | Aliases | Turkish | Subcommands |
|---|---|---|---|
| `/login` | `/l`, `/log` | `/giris` | |
| `/register` | `/reg` | `/kayit` | |
| `/changepassword` | `/changepass`, `/cp` | `/sifredegistir` | |
| `/logout` | | `/cikis` | |
| `/claim` | | `/sahiplen` | |
| `/premium` | | `/premium` | |
| `/cracked` | | `/crackedol` | |
| `/unregister` | | `/hesabisil` | |
| `/session` | | `/oturum` | `on` (`ac`), `off` (`kapat`) |
| `/captcha` | | `/dogrula` | |
| `/recover` | | `/kurtar` | |
| `/email` | | `/eposta` | `add` (`ekle`), `confirm` (`onayla`), `remove` (`kaldir`) |
| `/2fa` | | `/dogrulama` | `setup` (`kur`), `confirm` (`onayla`), `backup` (`yedek`), `email on`, `email off`, `off` (`kapat`) |
| `/uxmauth` | `/auth` | | `reload`, `version`, `doctor`, `info` (`bilgi`), `history` (`gecmis`), `perf` (`performans`), `database move`, `uuidscheme` (`uuidduzeni`), `forcelogin` (`zorlagiris`), `unban` (`yasakkaldir`), `unregister` (`kayitsil`), `setpassword` (`sifreata`), `forceregister` (`zorlakayit`), `disable2fa` (`dogrulamakapat`), `markpremium` (`premiumyap`), `markcracked` (`crackedyap`), `resolve` (`coz`), `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player`, `help` |

Names and aliases are the defaults from [commands.conf](../config/commands-conf.md); a change needs a restart. The
Turkish words work for anyone who types them and are listed to players who read Turkish.

## Who may run what

Every player node is given to everyone by default, so a player whose rank had one taken away still sees the command and
is told they may not use it. The `uxmauth.admin` nodes are operators only and hide their subcommands from everybody
else. Seven staff commands run only from the console unless `staff.in-game` lists them; see
[Staff commands](staff.md#console-only).

The Velocity jar registers the `/login` and `/register` words of `commands.conf` for the proxy's waiting world only.

Related: [Player commands](player.md), [Staff commands](staff.md)
