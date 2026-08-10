---
title: Compilation
description: Goauld component compilation
weight: 2
---


Some components require specific Go build tags:
- `client`: `-tags client`
- `agent-mini`: `-tags mini`
- `agent-dll`: `-tags dll`

> [!NOTE]
> `client` is required, not stylistic: it's the only build variant that embeds the full source tree used by `tealc compile` to build agents/servers on demand. Without it, `tealc compile` builds successfully but fails at runtime because there is no embedded source to compile from.

> [!NOTE]
> `agent-mini` is a small stager that fetches and executes the full agent at runtime.
>
> On connection, it downloads the full agent binary from the server over the encrypted control channel (in chunks), then runs it locally. This provides a second delivery path separate from the `/binaries/` download directory (see [server/agent downloading]({{< ref "03-server/05-agent_downloading" >}})).

The project can also be built using GoReleaser.

## Optional build dependencies

- `garble` for binary obfuscation ([https://github.com/burrowers/garble](https://github.com/burrowers/garble))
- `upx` for binary compression [https://github.com/upx/upx](https://github.com/upx/upx)
- `goreleaser` for automated builds [https://github.com/goreleaser/goreleaser](https://github.com/goreleaser/goreleaser)


## Agent

There are three ways to compile the agent. **Use the client CLI for most situations** as it handles compile-time configuration automatically.

### Using the client CLI (Recommended)
```bash
tealc compile --id agent --goarch amd64 --goos windows
```
Use this method to customize agent behavior with compile-time variables (see [general/variables]({{< ref "01-general/03-variables" >}})).

### Direct compilation
```bash
go build -o goauld ./agent
```
Use this for simple builds without customization.

### Using the wrapper script
```bash
go run ./scripts/build/ --id agent --goos windows --goarch amd64 --no-seed --gen-age-key=false --gen-access-token=false
```
Use this for fine-grained control over build parameters.

### Building as a Windows DLL

A DLL (Dynamic Link Library) can be loaded by other processes, useful for evasion scenarios. Build with the `dll` tag to create a loadable library instead of a standalone executable. This requires CGO and the `c-shared` build mode.

```bash
go build -tags dll -buildmode=c-shared -o goauld.dll ./agent
```

Using the client CLI, this corresponds to the `agent.dll` id (see [Compile using the client](#compile-using-the-client-recommended) below).

## Server
### Direct compilation
```bash
go build -o goauld_server ./server
```
### Using the wrapper script
```bash
go run ./scripts/build/ --gen-age-key=false --gen-access-token=false --id server --goos linux --goarch amd64 -vvv
```

## Client
### Direct compilation
```bash
go build -tags client -o tealc ./client
```
### Using the wrapper script
```bash
go run ./scripts/build/ --gen-age-key=false --gen-access-token=false --id client --goos linux --goarch amd64 -vvv
```

### Compile using the client (recommended)

```bash
tealc compile --id [client|server|agent|agent-mini|agent.dll]
```