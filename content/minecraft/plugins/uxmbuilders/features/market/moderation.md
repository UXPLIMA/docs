---
title: Market moderation
order: 3143
description: Player reports, listings hidden for review, and what staff can remove, restore, ban, refund and pay out.
icon: gavel
---

Players report listings; enough open reports hide one until a moderator looks at it. Staff with
`uxmbuilders.admin.market` decide from two menus or from commands: remove a listing, take back what it
sold, put it back, ban a seller from the market, refund a buyer or pay a seller by hand.

## Reports

`/builders market report <code> <reason> [note]`, with `uxmbuilders.market.report`:

| Reason | Typed as |
|---|---|
| Copied build | `copied_build` |
| Offensive content | `offensive_content` |
| Misleading | `misleading` |
| Spam | `spam` |
| Other | `other` |

A dash works in place of the underscore, and the note is the rest of the line, cut to
`reports.noteMax` characters.

| Refused when | Setting |
|---|---|
| It is your own listing | |
| You reported this listing before | Once per player and listing, ever |
| Your last report was too recent | `cooldowns.reportSeconds` |
| You made `reports.dailyLimit` reports in the last 24 hours | `0` is no limit |
| You have played less than `reports.minPlaytimeMinutes` | The server's own playtime statistic; `0` lets anyone report |

Staff online on that server with `uxmbuilders.market.notify` are told at once, with an **[Inspect]**
link. The `REPORTED` post goes to `moderationUrl` in [webhooks.yml](../../config/webhooks-yml.md) when
one is set, so the reporter's name stays out of the players' channel.

## Under review

The report that brings a listing's count to `reports.autoHideThreshold` hides it: it leaves the market,
nobody can buy it, and only its seller still sees it, marked **Under review**. The seller cannot put it
back on sale. A report only ever hides; what happens next is a moderator's decision. The count starts
again when the listing is restored. `0` turns hiding off.

## The menus

`/builders admin market reports [page]` lists the reported listings, the one waiting longest first.
Clicking one opens its inspection, also reached with `/builders admin market inspect <code>`:

| Button | Does |
|---|---|
| Seller | Their earnings owed and held, and their market ban |
| Reports | The reports, open and closed, with reason and note |
| Licences | Prints the active licences in chat, each with a refund link |
| Teleport | To where the design was captured, above the build |
| Restore | Shown while the listing is hidden or removed |
| Dismiss the reports | Closes every open report; the listing stays as it is |
| Remove | Asks for a reason, which the seller reads |
| Remove and revoke (shift-click) | Also takes back every licence |
| Ban the seller (shift-click) | Asks for a reason; the ban lasts until it is lifted |
| Lift the ban | Shown while the seller is banned |

## Commands

| Command | Does |
|---|---|
| `/builders admin market remove <code> [--revoke] [reason]` | Takes the listing down; `--revoke` also takes back every licence |
| `/builders admin market restore <code>` | Puts it back |
| `/builders admin market reports [page]` | The reports menu |
| `/builders admin market resolve <report> dismiss\|action` | Closes one report, by the id the reports list shows |
| `/builders admin market inspect <code>` | The inspection |
| `/builders admin market tp <code>` | Teleports to where it was captured |
| `/builders admin market ban <player> [duration] [reason]` | Bans a seller; a duration is `30m`, `12h`, `7d` or `2w`, none or `never` for good |
| `/builders admin market unban <player>` | Lifts the ban |
| `/builders admin market refund <purchase> [--revoke]` | Takes back one licence |
| `/builders admin market payout <player>` | Pays what the market owes a player now |

Every action works from the console except `tp`; `reports` and `inspect` print to chat there. A player
is found by name if they have played on the server, online or not.

## Removing and revoking

A removal closes the listing's open reports as acted on, frees the design to be listed again, and tells
the seller why, now or when they next join. The `REMOVED` webhook post names the moderator and the
reason.

Without `--revoke` buyers keep their licences: they paid, and the stored design does not change. With
it, each licence is taken back on its own:

| Step | What happens |
|---|---|
| The licence | Closed: it starts no new build, while a build already running carries on |
| The seller | Charged the net back through their earnings. Already paid, they owe it, and their next earnings pay it off first |
| The commission account | Charged its commission back, when it received one for that sale |
| The buyer | Refunded the price with `moderation.revokeRefund: FULL`; with `NONE` the licence is taken without a refund |

`refund <purchase>` does the same for one licence and always refunds the buyer; `refund <purchase>
--revoke` refunds only as `revokeRefund` says. The purchase id is the one the **Licences** links fill
in. A refund the economy refuses is written to the audit log for staff to pay by hand. A limited copy
taken back is not put on sale again.

## Restoring

A restore puts a hidden or removed listing back in the status it left, sold out if every copy went
meanwhile, dismisses its open reports, and tells the seller. It is refused as a duplicate when another
listing took the same design in the meantime. Licences a revoke took back stay taken back.

## Bans

A market ban stops a player from capturing and listing. In the same moment:

| | `market.yml` |
|---|---|
| What the market owes them is put on hold, as are sales made during the ban | `moderation.holdEarningsOfBanned` |
| Their listings are hidden (`HIDE`) or removed (`REMOVE`) | `moderation.onSellerBan` |

Buyers keep what they bought. Banning again replaces the earlier ban. `unban` releases the held earnings
to be paid; the listings stay hidden until a moderator restores each one. When a timed ban runs out, the
player may sell again, while the earnings it held stay on hold until `unban` or `payout`. `payout`
refuses while a ban runs, and lifts one that has run out on the way.

A player banned from the server is treated as blocked whatever the market says: their listings are left
out of the market, cannot be bought, and they are not paid until the server ban ends.

## Notes

- Moderators act without the player checks: no cooldowns, limits or fees. Every decision is written to
  the audit table when `logging.audit` is on.
- The seller and buyers a decision touches hear of it live, or from their mail when they next join.
- What a moderator types as a reason is shown to players as plain text.

Related: [Player market](index.md), [Selling a build](selling.md), [Permissions](../../permissions.md#market)
