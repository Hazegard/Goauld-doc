---
title: Shell logging
description: Goauld client SSH session logging
weight: 8
---

The client allows logging of SSH sessions to keep a record of commands executed and their outputs.

Log files are saved in the current directory with the following format: `[AGENT_NAME]-[CURRENT_DATE]_[CURRENT_TIME].log`.

Sessions are recorded in binary [ttyrec](https://en.wikipedia.org/wiki/Ttyrec) format (via [togettyc](https://github.com/hazegard/togettyc)), which is not human-readable. Open recorded sessions with `togettyc` or any ttyrec-compatible player to view the commands and output. The same logging mechanism is used by `scp`, `rsync`, `rclone`, and `jump --log`.

Example: `user@hostname1-2026-08-09_14-30-05.log`

> [!NOTE]
> If the session fails to authenticate, the log file is deleted rather than left on disk as a partial recording.

## Flag

- `--log`
