---
title: Event files
order: 53
description: What each events/<key>.yml holds, the announcements, and the placeholders they can use.
icon: file-text
---

Every event has a file, `events/<key>.yml`, written when you create the event in game. Items, the
name and the length are easiest to change from the manage menu; rewards and announcements are
written here. After editing, run `/sr reload`. On a network, edit it on the management server.

## The file

```yaml
id: "11111111-2222-3333-4444-555555555555"
event-key: "example-rush"
name: "Example Rush"
duration: "7d"
deliverables:
  - material: "DIAMOND"
    points: 10.0
  - material: "EMERALD"
    points: 25.0
rewards-explanation:
  - "<#FFC94D>First Place: <reward:1>"
start-broadcast: []
resume-broadcast: []
complete-broadcast: []
unpaid-broadcast: []
no-reward-broadcast: []
rewards:
  mode: "STATIC"
  ...
```

| Key | What it is |
|---|---|
| `id` | The event's identity. Never change it |
| `event-key` | The key, which must match the file name |
| `name` | The name players see |
| `duration` | The length of a run, such as `7d` or `12h30m`. A change here applies from the next run |
| `deliverables` | The items that count, each with its `points` per item |
| `rewards-explanation` | The lines the **Rewards** button shows |
| `...-broadcast` | The announcements, below |
| `rewards` | What each rank wins. See [Rewards](../features/rewards.md) |

An item added in game with a custom name or lore gets a `snapshot` line, which is how the plugin
recognises that exact item. Add such items in game rather than by hand.

## Announcements

Each announcement is a list of lines sent to every player online. Leave one empty (`[]`) and the
default text from `messages.yml` is used instead.

| Key | Sent when |
|---|---|
| `start-broadcast` | A new run starts |
| `resume-broadcast` | A paused run continues |
| `complete-broadcast` | A run ends and the rewards are paid |
| `unpaid-broadcast` | A run is stopped with **Stop Unpaid** |
| `no-reward-broadcast` | A run ends with nobody to reward |

| Placeholder | Works in | Shows |
|---|---|---|
| `<event_name>` | All | The event's name |
| `<end_time>`, `<time_left>`, `<duration>` | Start and resume | When the run ends, how long is left, the run's length |
| `<reward:N>` | All, and `rewards-explanation` | The `display` of rank N's reward |
| `<winner_N>` | The three end announcements | The player or players at rank N, 1 to 10 |
| `<winner_N_points>`, `<winner_N_items>` | The three end announcements | Their points, and the items they deposited |

Tied players appear together in `<winner_N>`, separated by `winner-separator` from `messages.yml`.
An empty rank shows `-`. Text uses MiniMessage colours, like `<#FFD166>` or `<bold>`.

## Deleting a file

Deleting a file does not delete the event: the plugin writes it back. To delete an event, use
`/sr delete`.

Related: [Rewards](../features/rewards.md), [Events and runs](../features/events-and-runs.md)
