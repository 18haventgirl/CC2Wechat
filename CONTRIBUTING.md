# Contributing to CC2Wechat

## Build

```bash
go build ./cmd/cc-connect/
```

## Test

```bash
go test ./core/... ./config/... ./daemon/...
```

## Lint

```bash
golangci-lint run ./...
```

## Code Style

- Follow standard Go conventions
- New agents and platforms implement the interfaces in `core/interfaces.go`
- Register via `core.RegisterAgent()` / `core.RegisterPlatform()`

## License

MIT License
