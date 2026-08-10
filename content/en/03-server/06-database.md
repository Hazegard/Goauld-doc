---
title: Database
description: Goauld server database
weight: 6
---

The server uses an SQLite database to store information regarding previously connected agents, including each agent's SSH keypair, which is used to authenticate a returning agent by ID.

Given that agents perform a full connection flow at each start, this history isn't strictly required for the tunnel to function — but if the on-disk database is disabled, previously-registered agent identities will not survive a server restart (the server falls back to an in-memory database that is discarded on exit).

### Flags

- `--no-db`: Disable the database on disk (switches to an in-memory database). Despite the flag name, this is the option that **disables** on-disk persistence — `--no-db` is currently a no-op, since the on-disk database is already enabled by default either way.
- `--db-file-name`: Path or filename of the SQLite file.