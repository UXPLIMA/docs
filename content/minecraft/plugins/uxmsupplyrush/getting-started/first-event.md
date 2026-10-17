---
title: Your first event
order: 13
description: Create an event, choose its items, give it rewards and start it.
icon: flag
---

Everything below happens in game, except the rewards, which you write in the event's file.

## 1. Create the event

Type `/sr events` and click **Create New Event**. The event is created as a draft with a default
name, and its manage menu opens. Click **Rename Event** and type the name players should see.

## 2. Add the items

With the manage menu open, click an item in your own inventory. It is added to the event, worth 1
point. Click the item in the menu and type its point value in chat.

| In the manage menu | What it does |
|---|---|
| Click an item in your inventory | Adds it to the event |
| Left-click an event item | Sets its point value |
| Right-click an event item | Removes it from the event |

Typing `!exit` or `cancel` in chat closes any open prompt.

## 3. Set the length

Click **Set Duration** and type how long a run lasts, for example `7d`, `12h30m` or `1w2d`. The
letters are `y` (year), `w` (week), `d` (day), `h` (hour), `m` (minute) and `s` (second).

## 4. Write the rewards

Open `plugins/uxmSupplyRush/events/<key>.yml`. The key is shown on the manage menu's info item,
and the **Copy Event ID** button prints it in chat. Fill in the `rewards` section, using
`events/example-rush.yml` as a model, then run `/sr reload`. See [Rewards](../features/rewards.md).

## 5. Start it

Click **Start Event**, or type `/sr start <key>`. The start announcement goes out, and players can open
`/sr` and hand in items until the time runs out.

## 6. Let it finish

When the time is up, the run ends by itself, the final leaderboard is announced and the winners
are paid. To end it early, use `/sr stop`, which asks whether to pay the rewards, end without
paying, or only pause. See [Events and runs](../features/events-and-runs.md).

Related: [Concepts](concepts.md), [Event files](../config/event-files.md)
