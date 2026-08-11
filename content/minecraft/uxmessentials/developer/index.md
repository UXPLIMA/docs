---
title: Developer API
order: 161
description: Overview, Adding Dependency, Events, Query API, Action API, Menu API and REST API.
---

- [Overview](overview.md): uxmEssentials is built to be a good neighbour. Your plugin can watch what it does, refuse the things it is about to do, extend its menus, and read its economy, through a published API you compile against like any other library.
- [Adding Dependency](dependency.md): The uxmEssentials developer API is published as an ordinary Maven artifact. You add one repository and one coordinate, and your IDE gives you the event classes, the front door, and the value types with full javadoc.
- [Events](events.md): Everything uxmEssentials does is published as an ordinary Bukkit event, and the operations that can be refused cleanly are published twice: once as a question you may cancel, once as the fact afterwards.
- [Query API](queries.md): Events tell you what uxmEssentials did. Queries answer what is true right now: how many homes a player owns, what they hold, whether they are banned, who is away, which worlds are loaded.
- [Action API](actions.md): Queries answer what is true. Actions change it: set a home, pay a player, hand over a kit, ban somebody, each running the same use case the matching command runs.
- [Menu API](menu-api.md): The menu engine behind every GUI in uxmEssentials is extensible: teach it your own actions, requirements, placeholders, list sources and icons.
- [REST API](rest-api.md): HTTP and WebSocket access to uxmEssentials for the programs that are not plugins: a panel, a bot, a store webhook, a script.
