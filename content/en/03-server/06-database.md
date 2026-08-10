---
title: Database
description: Goauld server database
weight: 6
---

## Overview

The server uses an SQLite database to store three categories of agent information:

**SSH keypair**: Each agent has a unique SSH keypair generated when first registered. This allows the server to authenticate returning agents by ID without requiring password storage.

**One-time password (OTP)**: The server generates and atomically rotates an OTP for each agent's SSH password-based reconnection path. This limits the window for password-based attacks.

**Agent metadata**: Name, used ports, remote-port-forwarding state, connection mode, hostname, platform, and IP addresses. This information is used across the management and admin APIs to track agent state and capabilities.

Agents perform a full connection flow at each start, so database history isn't strictly required for tunnel operation. However, if the on-disk database is disabled, previously-registered agent identities will not survive a server restart. In this case, the server falls back to an in-memory database that is discarded on exit.

> [!NOTE]
> On every startup, regardless of whether the database is on disk or in-memory, the server resets each agent's dynamic connection state: connection mode is cleared, used ports and remote-port-forwarding state are cleared, and every agent is marked disconnected. Agent identities themselves are not affected: agents reconnect and re-register automatically, restoring this state.

### Flags

- `--no-db`: Disable the database on disk (switches to an in-memory database that is discarded on exit).
- `--db-file-name`: Path or filename of the SQLite file.