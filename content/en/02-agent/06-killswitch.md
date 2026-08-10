---
title: Killswitch
description: Goauld agent killswitch
weight: 6
---

The agent automatically shuts down after a specified number of days to prevent indefinite operation.

## Flag

- `--kill-switch`: Set the number of days before exiting (default: `7`; `0` disables the killswitch entirely)

> [!NOTE]
> The timer counts from the agent's execution time. If restarted by external systems (scheduled tasks, cron jobs), the timer resets.