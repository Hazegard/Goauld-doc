---
title: Killswitch
description:
weight: 6
---

To ensure that no agent runs indefinitely, a killswitch has been implemented. After a specified amount of days, the agent automatically shuts down.

## Flag

- `--kill-switch`: Set the number of days before exiting

> [!NOTE]
> The timer counts from the agent's execution time.
> Consequently, if an external system (scheduled task, cron job, etc.) restarts the agent, the killswitch timer resets.