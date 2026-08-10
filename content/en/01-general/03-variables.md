---
title: Variables
description: Configuration variable sources
weight: 3
---

## Passing variables to components (agent, client & server)

Components can retrieve configuration parameters from several sources.
The following order defines the precedence (from highest to lowest):

1. Command-line argument (`--var=1`)
2. Configuration file passed through command line (`--config-file`)
3. Environment variable: uppercase the flag name, replace dashes with underscores, and prefix it.
   - Agent and server: `GOAULD_` (both share the same namespace, e.g. `GOAULD_AGE_PUBKEY=...`, `GOAULD_HTTP_DOMAIN=...`)
   - Client: `TEALC_` (e.g. `TEALC_ACCESS_TOKEN=...`)
4. Default configuration file (see [general/configuration file]({{< ref "01-general/04-configuration_file" >}}))
5. The flag's default value, as set in source, optionally overridden at build time via `-ldflags` or `tealc compile --env` (see [general/compilation]({{< ref "01-general/02-compilation" >}}))

> [!NOTE]
> Variables of the form `AGENT__AGE_PUBKEY`, `SERVER__HTTP_DOMAIN`, `CLIENT__...`, etc. are a separate, compile-time-only mechanism: they are read from an env file (via `tealc compile --env`) and baked into the binary as build-time defaults (tier 5), not read from the process environment at runtime. Setting one of these as a shell environment variable before running the compiled binary has no effect.

> [!NOTE]
> The client's `-P`/`--password` flag is an exception to the order above: its environment variable (`TEALC_PASSWORD`, tier 3) takes precedence over an explicit `--config-file` value (tier 2), the reverse of every other flag. This is required for `tealc ssh` (and the other commands that authenticate against an agent) to work correctly: `tealc` re-invokes itself as `SSH_ASKPASS` using `TEALC_PASSWORD` to carry a freshly prompted or otherwise runtime-resolved password, and that value must not be overridden by a stale `--config-file` entry.

