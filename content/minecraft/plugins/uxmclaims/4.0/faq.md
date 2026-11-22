---
title: FAQ
order: 190
description: Short answers to the questions server owners ask most about uxmClaims 4.
icon: circle-help
---

## Installing

**The plugin disables itself on the first start.** That is expected: `license.key` in `config.yml`
is empty. Set it and start again. See [Installation](getting-started/installation.md).

**It says it needs Java 21.** uxmClaims 4 runs on Java 21 or newer. On Minecraft 1.19.4, plain Spigot
refuses Java 21; use Paper there.

**Do I need a separate jar for Folia?** No. One jar runs on Spigot, Paper and Folia. Every shipped
module is marked Folia safe.

**Which database should I use?** SQLite is fine for one server. Use MySQL, MariaDB or PostgreSQL when
you want backups on a database server or [network mode](network-mode.md). See
[Database](configuration/database.md).

## Modules and presets

**How do I turn a module on?** Set it to `true` in `plugins/uxmClaims/modules.yml` and restart.
`/uxm modules` then shows whether it started and, if not, why.

**A module I turned on shows as off.** Read the reason in `/uxm modules`. The usual ones: a module it
requires is off (Nations needs Towns), or the plugin it needs is missing (Economy needs Vault, Bedrock
needs Floodgate or Geyser).

**Does turning a module off delete its data?** No. Its tables and its folder stay. Turn it on again
and the data is back.

**Can I change the preset later?** Yes. `/uxm setup` or `/uxm preset apply <id>` at any time. A
preset only writes `modules.yml` and a few values, keeps a backup, and never touches lands. See
[Presets](configuration/presets.md).

**Can I make my own preset?** Yes: put a preset file in `plugins/uxmClaims/presets/`. The format is
on [Presets](configuration/presets.md).

## Upgrading from 3.x

**Will my claims survive the upgrade?** Yes. Claims, roles, members, flags, warps, bans, vault
contents and settings move to 4.0 on its first start. Try it first with a dry run, and keep the 3.x
backup. See [Upgrading from 3.x](upgrading/from-3x.md).

**I run several 3.x servers on one database.** Move one server first and let it finish. A second
server that then starts on its own 3.x folder sees the moved data and only converts its own config,
language and menu files (`CONFIG_ONLY` in its report); it copies and deletes nothing. See
[Upgrading from 3.x](upgrading/from-3x.md).

**Can I go back to 3.x?** Yes, with the rollback steps on [Rollback to 3.x](upgrading/rollback.md).
Anything created after the move to 4.0 is lost when you go back.

**My add-on was built for the 3.x API.** Keep [Legacy API](modules/legacy-api.md) on and it keeps
working. The bridge goes away in 5.0, so ask the author to move to the 4.0 API; the mapping is on
[Migrating from the 3.x API](developer/legacy-api.md).

**My 3.x placeholders stopped working.** They work while `legacy-paths` is on in
`modules/placeholders/config.yml`. See [Placeholders](placeholders.md).

**Do admins still bypass protection?** A server moved from 3.x keeps the 3.x behaviour:
`protection.admin-bypass` in `modules/lands/config.yml` is set to `IMPLICIT`, and `uxmclaims.admin`
always bypasses. A new install starts with `TOGGLE`: staff with `uxmclaims.admin.bypass` turn it on
with `/uxm bypass on` and off with `/uxm bypass off`. See [Behaviour changes](upgrading/behaviour-changes.md).

## Players and permissions

**Nobody can claim.** Check that the world is protected in `modules/lands/worlds.yml`, and that the
player has the `/claim` command nodes. With LuckPerms and `undefined-ability: deny`, also grant
`uxmclaims.ability.*` to your default group. See [Permissions](permissions.md).

**How do I give a rank more chunks?** Grant `uxmclaims.limit.chunk.<n>`, for example
`uxmclaims.limit.chunk.50`. How several nodes add up is set in `modules/limits/limits.yml`. See
[Limits](modules/limits.md).

**How do I make claiming cost money?** Turn on Vault and an economy plugin, then set the prices in
`modules/economy/costs.yml`. See [Economy](modules/economy.md).

**Can each player see the plugin in their own language?** Set `language.per-player: true` in
`config.yml` and list the languages in `language.enabled`. Eight languages ship with 4.0: English,
Turkish, German, Spanish, French, Polish, Brazilian Portuguese and Russian. Some
texts, such as holograms, signs and town lists, stay in the server language. See
[Languages](configuration/languages.md).

**I edited a message and the update did not change it.** That is on purpose. Texts that may be your
edits are kept at an update; `/uxm lang update` lists them and replaces them on request.

## Towns and nations

**Can players have personal lands and towns at the same time?** Yes. `personal-lands.enabled` in
`modules/lands/config.yml` decides whether players may claim on their own; towns own their lands
next to them. The `towny-economy` and `war-server` presets turn personal lands off.

**Can lands join a nation without a town?** Not yet. Nations need towns, so the `lands-style`
preset's nations and levels stay off with a warning until towns run.

**Where does tax money go?** Into the town's or nation's bank in the [Bank](modules/bank.md)
module. Taxes and upkeep run once a day on the [Taxes](modules/taxes.md) module's new-day time.

## Bedrock and other platforms

**Do Bedrock players get forms?** Yes, with Floodgate or Geyser on the server and the
[Bedrock](modules/bedrock.md) module on. Menus, text input and confirmations become forms.

**Do the menus use the new dialog screens?** On servers with a dialog API, yes, through
[Dialogs](modules/dialogs.md). Elsewhere, menus stay chests and input stays in chat.

## Running several servers

**Can two servers share one database?** Only in [network mode](network-mode.md). Without it, the
second server is refused at start-up to protect the data.

**Is a land vault shared between servers?** No. In network mode vaults open on one server only, the
one named in `vault-node`.

Related: [Getting Started](getting-started/index.md), [Upgrading from 3.x](upgrading/from-3x.md), [Modules](modules/index.md)
