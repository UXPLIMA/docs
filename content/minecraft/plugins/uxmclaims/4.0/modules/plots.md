---
title: Plots
order: 1326
description: Areas with a plot type that land owners and towns sell or rent out, with signs and a market menu.
icon: store
---

A plot is an area with a plot type. Whoever manages the land puts it up for sale or for rent, and players
buy or rent it with `/plot buy`, `/plot rent`, a plot sign or the market menu. Rent is collected once a
day, and a bought plot in a town pays a daily plot tax to the town bank.

Module `plots` · disabled by default · `modules/plots/config.yml` · requires [Areas](areas.md)

## Commands

Every `/plot` command works on the area you stand in, except `market`.

| Command | What it does | Permission |
|---|---|---|
| `/plot`, `/plot info` | Shows the plot you stand in: type, status, owner, price, rent and tenant. | `uxmclaims.command.plots.use` |
| `/plot menu` | Opens the menu of the plot you stand in. | `uxmclaims.command.plots.use` |
| `/plot market` | Opens the list of plots for sale and for rent. | `uxmclaims.command.plots.use` |
| `/plot set type <type>` | Gives the area a plot type, which makes it a plot and writes the type's flags to it. | `uxmclaims.command.plots.use` |
| `/plot forsale <price>` | Puts the plot up for sale. | `uxmclaims.command.plots.use` |
| `/plot forrent <price> <period> [max]` | Offers the plot for rent: the price per period (such as `7d`) and the most periods a tenant may pay ahead. | `uxmclaims.command.plots.use` |
| `/plot notforsale` | Takes the plot off the market. | `uxmclaims.command.plots.use` |
| `/plot buy` | Buys the plot. | `uxmclaims.command.plots.use` |
| `/plot rent` | Rents the plot for one period. | `uxmclaims.command.plots.use` |
| `/plot extend` | Pays one more rent period ahead. | `uxmclaims.command.plots.use` |
| `/plot autorenew <on\|off>` | Turns the automatic renewal of your rental on or off. | `uxmclaims.command.plots.use` |
| `/plot unrent` | Gives the plot you rent back. | `uxmclaims.command.plots.use` |
| `/plot evict` | Puts the tenant out. An admin can also take a bought plot back from its buyer. | `uxmclaims.command.plots.use` |
| `/plot remove` | Makes the plot an ordinary area again. | `uxmclaims.command.plots.use` |
| `/town plots` | Opens your town's plots that are for sale or for rent. Only while [Towns](towns.md) runs. | `uxmclaims.command.plots.use` |
| `/town set plottax <amount\|none>` | Sets the daily tax buyers of your town's plots pay; `none` goes back to the plot types' tax. Only while Towns runs. | `uxmclaims.command.plots.plottax` and the town permission `plots:tax` |

Setting the type, selling, renting out, evicting and removing need the land role permission `plots:sell`
(the land owner side always has it). A plot's buyer also manages their own plot. The local names of
these commands are in `modules/plots/lang/<language>/commands.yml`.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.plots.use` | everyone | `/plot`, `/town plots`. |
| `uxmclaims.command.plots.plottax` | everyone | `/town set plottax`; the town rank also needs `plots:tax`. |
| `uxmclaims.ability.plot.settype` | everyone | Setting the type of plots you manage. |
| `uxmclaims.ability.plot.sell` | everyone | Putting plots up for sale where the land role allows it. |
| `uxmclaims.ability.plot.rent` | everyone | Offering plots for rent where the land role allows it. |
| `uxmclaims.ability.plot.buy` | everyone | Buying and renting plots. |
| `uxmclaims.ability.plot.evict` | everyone | Putting tenants out of plots you manage. |
| `uxmclaims.admin.plots` | op | Managing every plot, taking bought plots back, no plot limit. |

| Role permission | Given to | Grants |
|---|---|---|
| `plots:sell` | Owner | Selling and renting out the land's areas as plots. |
| `plots:tax` | Owner | Setting the town's plot tax. |

| Limit | Default | What it caps |
|---|---|---|
| `uxmclaims.limit.plot` | `5` | Plots a player owns or rents together. The highest value a player has counts. |

## Settings

| Key | Default | What it does |
|---|---|---|
| `types` | `default`, `shop`, `embassy`, `arena`, `farm`, `inn`, `bank` | The plot types. Each has `flags` (area flag key to `ALLOW` or `DENY`, written to the area when it gets the type) and `tax`, the daily plot tax a buyer pays in a town. `shop` taxes 5, `embassy` 10, `arena` allows `lands:pvp`. |
| `roles.owner` | `manager` | The area role a buyer gets, by name. `manager` lets them run the area's members and flags. |
| `roles.tenant` | `member` | The area role a tenant gets. A name the area does not have gives its member role. |
| `rent.max-period` | `30d` | The longest period one rent payment may buy. |
| `rent.max-periods` | `4` | The most periods a tenant may have paid ahead, unless the offer sets fewer. |
| `rent.auto-renew` | `true` | Whether a new rental renews on its own when its paid time runs out. |
| `rent.notify-before` | `1d` | Tells an online tenant when the paid time ends within this long. |
| `signs.enabled` | `true` | Whether plot signs work. |
| `signs.header` | `[plot]` | The first line that makes a sign a plot sign, in any case. |
| `signs.hologram` | `true` | Also shows a hologram above plot signs while [Holograms](holograms.md) runs. |
| `plot-tax.enabled` | `true` | Whether bought plots in towns pay the daily plot tax. |
| `plot-tax.unpaid` | `REVOKE` | `REVOKE` takes the plot back from a buyer who cannot pay; `WARN` only tells them. |
| `new-day-time` | `00:00` | The server time (HH:mm) rents and plot tax are collected when [Taxes](taxes.md) is off. With Taxes on, they follow its new day. |
| `warn-plot-squared` | `true` | Warns at start-up when PlotSquared is installed, since both use `/plot`. |

## Where the money goes

A price or rent goes to the plot's buyer when it has one. Otherwise it goes to the bank of the town or
nation that owns the land, while [Bank](bank.md) runs for that holder, and else to the land's owner.

A sign whose first line is `[plot]`, placed inside a plot by someone who manages it, turns into a plot
sign that shows the price and sells or rents the plot on a click.

## Placeholders

| Placeholder | Renders |
|---|---|
| `%uxmclaims_plots_market%` | How many plots are on the market. |
| `%uxmclaims_plots_owned%` | How many plots the player bought. |
| `%uxmclaims_plots_rented%` | How many plots the player rents. |
| `%uxmclaims_plots_here_name%` | The name of the plot the player stands in. |
| `%uxmclaims_plots_here_type%` | Its plot type. |
| `%uxmclaims_plots_here_status%` | Its status: For sale, For rent, For sale or rent, Rented, Owned or Not on the market. |
| `%uxmclaims_plots_here_price%` | Its sale price. |
| `%uxmclaims_plots_here_rent%` | Its rent per period. |
| `%uxmclaims_plots_here_owner%` | Its buyer. |
| `%uxmclaims_plots_here_tenant%` | Its tenant. |

The `here_` placeholders are empty when the player is offline or not in a plot.

## Notes

- **PlotSquared also uses `/plot`.** Give the root a local name such as `parcel` in `commands.yml`
  (`plot.label`); a restart applies it.
- **The town plot tax needs the bank.** It is paid into the town bank, so without [Bank](bank.md) running
  with `holders.town` on, no plot tax is collected; the log says so once.
- **The area is not reset when a rental ends.** The tenant's builds stay; restoring an area is not done yet.
- **No plot groups, auctions or instalments yet.** Each plot is sold or rented on its own, paid in one go.
- **Membership is a second step.** The buyer or tenant is added to the area after the plot is saved; if
  that step fails it is only logged, and you add them by hand.

Related: [Areas](areas.md), [Towns](towns.md), [Bank](bank.md), [Taxes](taxes.md), [Economy](economy.md)
