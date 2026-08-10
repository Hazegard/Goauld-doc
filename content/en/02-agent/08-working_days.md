---
title: Working days
description: Agent working days feature
weight: 8
---


During assessments, it may be necessary to keep an agent running for several days, since restarting it remotely (redeploying and re-executing the payload) can be difficult.

This feature allows the agent to pause all network activity at a specified time and automatically resume at a specified hour, reducing network footprint.

**Behavior:**
- If the agent starts outside working hours, it waits until the next start time
- Saturdays and Sundays are always non-working days (cannot be configured)
- If the agent would start on a weekend, it defers to the next Monday


## Flags

- `--only-working-days`: to enable the working days feature
- `--working-day-start` : the given hour when the agent resumes network connection (format: `[HOURS]:[MINUTES]`, 24-hour format)
- `--working-day-end`: the given hour when the agent stops all network connection (format: `[HOURS]:[MINUTES]`, 24-hour format)
- `--working-day-timezone`: the timezone to use

## Keeping the system awake

The agent can also prevent the host from sleeping or locking while it is running, which helps keep it reachable during the working period.

- `--keep-awake`: prevent the system from sleeping or locking.

