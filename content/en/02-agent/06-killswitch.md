---
title: Killswitch
description: Goauld agent killswitch
weight: 6
---

To ensure that no agent runs indefinitely, a killswitch has been implemented. After a specified number of days, the agent automatically shuts down.

## Flag

- `--kill-switch`: Set the number of days before exiting (default: `7`; `0` disables the killswitch entirely)

> [!NOTE]
> The timer counts from the agent's execution time.
> Consequently, if an external system (scheduled task, cron job, etc.) restarts the agent, the killswitch timer resets.