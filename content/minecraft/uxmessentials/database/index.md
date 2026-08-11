---
title: Database
order: 156
description: SQLite (Default), MySQL / MariaDB and PostgreSQL.
---

- [SQLite (Default)](sqlite.md): uxmEssentials ships with an embedded SQLite database and uses it out of the box. There is nothing to install, nothing to run alongside your server, and no credentials to manage. The first time the plugin starts it creates the database file and builds its own schema. For the large majority of single servers this is all you will ever need.
- [MySQL / MariaDB](mysql.md): When one server outgrows SQLite (or when you want several servers to share the same data) switch the storage backend to MySQL or MariaDB. Both speak the MySQL wire protocol, so a single backend = "mysql" setting drives either one. This is a first-class, fully tested path (the same queries and migrations run on every backend), not a fallback.
- [PostgreSQL](postgresql.md)
