---
title: Taxes
order: 1324
description: The daily economy run that collects resident tax, nation tax and upkeep, and what happens to holders that cannot pay.
icon: receipt
---

The taxes module runs one economy day every day at a set time. On that day residents pay their town, towns pay
their nation, and towns and nations pay upkeep to the server from their [Bank](bank.md). Personal lands can
pay upkeep and collect a member tax too, both off by default.

Module `taxes` · disabled by default · `modules/taxes/config.yml` · requires [Bank](bank.md), [Lands](lands.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/town set taxes <amount>` | Sets the daily tax your residents pay, a sum or a percent. | `uxmclaims.command.taxes.use` |
| `/town toggle taxpercent` | Switches the resident tax between a flat sum and a percent of each resident's balance. | `uxmclaims.command.taxes.use` |
| `/town upkeep` | Shows your town's daily upkeep, its debt and the next economy day. | `uxmclaims.command.taxes.use` |
| `/nation set taxes <amount>` | Sets the daily tax member towns pay, a sum or a percent. | `uxmclaims.command.taxes.use` |
| `/nation toggle taxpercent` | Switches the town tax between a flat sum and a percent of the town bank. | `uxmclaims.command.taxes.use` |
| `/nation upkeep` | Shows your nation's daily upkeep, its debt and the next economy day. | `uxmclaims.command.taxes.use` |
| `/claim upkeep` | Shows the upkeep and debt of the land you stand in. Only with `land-upkeep.enabled`. | `uxmclaims.command.taxes.use` |
| `/claim tax` | Shows the daily tax members of the land you stand in pay. Only with `land-member-tax.enabled`. | `uxmclaims.command.taxes.use` |
| `/claim tax set <amount>` | Sets that tax. The land's owner, or a member whose land role has `taxes:edit`. | `uxmclaims.command.taxes.use` |
| `/uxm taxes newday` | Runs a full economy day now. | `uxmclaims.admin.taxes` |
| `/uxm taxes preview` | Shows what the next economy day would charge. | `uxmclaims.admin.taxes` |

Setting a tax or switching percent mode needs the mayor or nation leader, or a rank with `taxes:edit`.
Switching percent mode sets the rate back to 0, so set it again afterwards.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.taxes.use` | everyone | The tax and upkeep commands under `/town`, `/nation` and `/claim`. |
| `uxmclaims.bypass.taxes` | nobody | Paying no resident tax and no land member tax. |
| `uxmclaims.admin.taxes` | op | Setting any holder's taxes, `/uxm taxes newday` and `/uxm taxes preview`. |

| Role permission | Given to | Grants |
|---|---|---|
| `taxes:edit` | owner | Setting the land's, town's or nation's taxes. Members, residents or ranks that have it also hear about upkeep and debt. |

## Settings

| Key | Default | What it does |
|---|---|---|
| `schedule.new-day-time` | `'12:00'` | When the economy day starts, `HH:mm` on the wall clock. |
| `schedule.timezone` | `''` | Time zone such as `Europe/Istanbul`. Empty uses the server's. |
| `schedule.catch-up-missed-days` | `false` | After downtime, run every missed day (`true`) or only the latest (`false`). |
| `schedule.max-catch-up-days` | `7` | Most missed days run at once. |
| `town.resident-tax.enabled` | `true` | Residents pay their town. |
| `town.resident-tax.default` | `0` | Tax a new town charges. |
| `town.resident-tax.max` | `1000` | Highest flat tax a town may set. |
| `town.resident-tax.percent-cap` | `25` | Highest percent a town may set in percent mode. |
| `town.resident-tax.remove-unpaid` | `false` | Remove residents who cannot pay (`true`) or only warn them. |
| `town.upkeep.flat` | `10` | Daily town upkeep. |
| `town.upkeep.per-chunk` | `0` | Added per claimed chunk. |
| `town.upkeep.per-outpost` | `5` | Added per town land after the first. |
| `town.upkeep.grace-days` | `3` | Days a new town pays no upkeep. |
| `nation.town-tax.enabled` | `true` | Member towns pay their nation, from the town bank. |
| `nation.town-tax.default` | `0` | Tax a new nation charges. |
| `nation.town-tax.max` | `1000` | Highest flat tax a nation may set. |
| `nation.town-tax.percent-cap` | `100` | Highest percent of the town bank a nation may set in percent mode. |
| `nation.town-tax.remove-unpaid` | `false` | Remove towns that cannot pay from the nation (`true`) or only warn. |
| `nation.upkeep.flat` | `100` | Daily nation upkeep. |
| `nation.upkeep.per-town` | `0` | Added per member town. |
| `nation.upkeep.grace-days` | `3` | Days a new nation pays no upkeep. |
| `bankruptcy.enabled` | `false` | Whether a holder that misses upkeep goes bankrupt and gets `debt-cap-days` to pay. |
| `bankruptcy.debt-cap-days` | `7` | Unpaid days before `on-cap` applies, with bankruptcy on. |
| `bankruptcy.on-cap` | `UNCLAIM` | `UNCLAIM` takes one outer chunk a day, `RUIN` turns the holder into a ruin (needs [Ruins](ruins.md)), `DELETE` deletes it. |
| `land-upkeep.enabled` | `false` | Personal lands pay upkeep from their land bank. Needs `holders.land` in the bank config. |
| `land-upkeep.flat` | `0` | Daily land upkeep. |
| `land-upkeep.per-chunk` | `1` | Added per claimed chunk. |
| `land-upkeep.grace-days` | `3` | Days a new land pays no upkeep. |
| `land-member-tax.enabled` | `false` | Members of a personal land pay its land bank. Needs `holders.land` in the bank config. |
| `land-member-tax.default` | `0` | Tax a land charges until its owner sets one. |
| `land-member-tax.max` | `1000` | Highest tax an owner may set. |
| `land-member-tax.remove-unpaid` | `false` | Untrust members who cannot pay (`true`) or only warn them. |
| `batch-size` | `20` | Payments handled before the server gets a short break. |

## How unpaid upkeep works

Upkeep and any old debt are taken from the bank together. When the bank cannot cover the whole sum, nothing is
taken: the day's upkeep is added to the debt and the people who run the holder are told. The next day that the
bank can pay upkeep plus debt clears it.

<Callout type="warning" title="Bankruptcy is off by default">

With `bankruptcy.enabled: false`, `on-cap` applies after the first unpaid day, and it applies again every day
the debt stays. Out of the box that is one chunk unclaimed per unpaid day. Turn bankruptcy on to give holders
`debt-cap-days` to pay first.

</Callout>

A resident or town tax that cannot be paid is never a debt: the payer is warned, or removed when
`remove-unpaid` is on.

## Notes

- The first start with the module on only records a starting day; taxes begin with the next economy day.
- A day interrupted by a crash or restart is finished on the next start without charging anyone twice.
- [Levels](levels.md) can change a holder's upkeep by a percent.
- The town plot tax is set and collected by [Plots](plots.md), not here.
- A holder that stays at the debt cap is charged every day and its managers get the penalty notice every day.

Related: [Bank](bank.md), [Towns](towns.md), [Nations](nations.md), [Ruins](ruins.md), [Inbox](inbox.md)
