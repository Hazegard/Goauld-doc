---
title: Variables
description: Configuration variable sources
weight: 3
---

## Passing variables to components (agent, client & server)

Components can retrieve configuration parameters from several sources.
The following order defines the precedence (from highest to lowest priority):

1. Command-line argument (`--var=1`)
2. Configuration file passed through command line (`--config`)
3. Environment variable (e.g. `VAR=1`)
4. Default configuration file (see [general/configuration file]({{< ref "01-general/04-configuration_file" >}})
5. Compile time defined value (`-ldflags`, or compiling with `tealc compile --env`: [general/compilation]({{< ref "01-general/02-compilation" >}}#compile-using-the-client))
6. Hardcoded default value

