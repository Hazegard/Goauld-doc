---
title: Database
description:
weight: 6
---

The server uses an SQLite database to store information regarding previously connected agents.

Given that agents perform a full connection flow at each start, the data stored in the database isn't required.

## Flag:

- `--no-db` Disable the database on disk.
- `--db-file-name`   Path or filename of the SQLite file .