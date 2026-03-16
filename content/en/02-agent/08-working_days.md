---
title: Working days
description: Agent working days feature
weight: 8
---


During assessments, it may be necessary to keep an agent running for several days (it can be hard to execute it). 

To reduce the agent's network footprint, a mechanism allows it to pause all network activity at a specified hour and automatically resume at another specified hour.

If the agent starts outside the working period, it will wait until the next start hour.


## Flags

- `--only-working-days`: to enable the working days feature
- `--working-day-start` : the given hour when the agent resumes network connection (format: `[HOURS]:[MINUTES]`, 24-hour format)
- `--working-day-end`: the given hour when the agent stops all network connection (format: `[HOURS]:[MINUTES]`, 24-hour format)
- `--working-day-timezone`: the timezone to use

