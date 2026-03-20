---
title: Database
description:
weight: 6
---

THe servers uses an SQLite database to store informations regarding previously connected agents.

GIven that agent perform a full connection flow at each start, the data stored in the database isn't required.

## Flag:

- `--no-db` Disable the database on disk.
- `--db-file-name`   Path or filename of the SQLite file .