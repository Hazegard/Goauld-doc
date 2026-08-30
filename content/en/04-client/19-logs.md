---
title: Logs
description: Goauld client retrieval of recorded agent logs
weight: 19
---

## Retrieve agent logs

`tealc logs [AGENT_NAME]` pulls the log lines recorded in the agent's in-memory buffer.

```bash
tealc logs [AGENT_NAME]
```

## Snapshot semantics

The pull is a frozen snapshot taken the moment the agent receives the request. Concurrent logging on the agent does not reorder, evict, or duplicate entries mid-pull, so what you see is a consistent view of the buffer at that instant.

## Flags

- `--format`: output format, `text` (default) or `json`.
- `--output`: write logs to a file instead of stdout.
- `--level`: filter to a single log level: `trace`, `debug`, `info`, `warn`, `error`, or one of the custom levels `run`, `kill`, `reset`.
- `--limit`: maximum number of entries to return, most recent first. `0` (default) means unlimited.
- `--tail`: after printing the initial snapshot, keep the connection open and print new entries as the agent logs them. Press Ctrl+C to stop.

> [!NOTE]
> `--level` and `--limit` are applied on the agent side, before the entries are sent back. Filtering a large buffer down to one level or the last few entries only transfers what matches, it does not pull the full buffer and filter it locally.

## Examples

```bash
# Everything currently buffered, as text
tealc logs myagent

# Only warning-level entries
tealc logs myagent --level warn

# Last 50 entries, as JSON, written to a file
tealc logs myagent --limit 50 --format json --output myagent.jsonl

# Follow new entries live
tealc logs myagent --tail

# Follow only error-level entries live
tealc logs myagent --level error --tail
```

> [!NOTE]
> Entries are ordered chronologically down to the millisecond. Lines logged within the same second still come out in the order they were actually written.

> [!NOTE]
> If log recording is disabled on the agent, the command fails with an explicit error rather than returning an empty result.
