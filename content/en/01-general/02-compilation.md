---
title: Compilation
description:
weight: 2
---


Some components require specific Go build tags:
- `client`: `-tags client`
- `agent-mini`: `-tags mini`

The project can also be built using GoReleaser.

## Optional build dependencies

- `garble` for binary obfuscation ([https://github.com/burrowers/garble](https://github.com/burrowers/garble))
- `upx` for binary compression [https://github.com/upx/upx](https://github.com/upx/upx)
- `goreleaser` for automated builds [https://github.com/goreleaser/goreleaser](https://github.com/goreleaser/goreleaser)


## Agent

### Direct compilation
```bash
go build -o goauld ./agent
```
### Using the wrapper script
```bash
go run ./scripts/build/  --id agent --goos windows --goarch amd64 --no-seed --gen-age-key=false --gen-access-token=false
```

### Using the client CLI
```bash
tealc compile --id agent --goarch amd64 --goos windows
```

> [!NOTE]
> Compiling with the CLI is recommended as it allows passing compile-time variables to the agent (see [general/variables]({{< ref "01-general/03-variables" >}}))

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