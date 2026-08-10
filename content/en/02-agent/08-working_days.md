---
title: Working days
description: Agent working days feature
weight: 8
---


During assessments, it may be necessary to keep an agent running for several days (it can be difficult to execute). 

To reduce the agent's network footprint, a mechanism allows it to pause all network activity at a specified time and automatically resume later at a specified hour.

If the agent starts outside the working period, it will wait until the next start hour.

Saturdays and Sundays are always treated as non-working days, regardless of the configured start/end hours, and there is currently no flag to change which days count as working days. If the agent would otherwise start during the configured hours on a Saturday or Sunday, it is deferred all the way to the next Monday — not just "later today" or "tomorrow".


## Flags

- `--only-working-days`: to enable the working days feature
- `--working-day-start` : the given hour when the agent resumes network connection (format: `[HOURS]:[MINUTES]`, 24-hour format)
- `--working-day-end`: the given hour when the agent stops all network connection (format: `[HOURS]:[MINUTES]`, 24-hour format)
- `--working-day-timezone`: the timezone to use

