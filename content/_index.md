---
title: go-flagged
---

`go-flagged` is a Go library for reflection-driven flag binding: `Bind(set *flag.FlagSet, target any) error` walks a struct and registers each field as a flag on the set, deriving flag names from the struct's fields (nested structs contribute a name prefix) and using each field's current value as the flag's default.

The library dispatches on each field's concrete type, rejects a name already present on the set, and reports every failure as a matchable sentinel error (`errors.Is`), never a formatted string.

## Install

```sh
go get github.com/gomatic/go-flagged
```

## Use

```go
var config struct {
	Verbose bool
	Server  struct {
		Port int
	}
}

set := flag.NewFlagSet("app", flag.ContinueOnError)
if err := flagged.Bind(set, &config); err != nil {
	// handle binding errors (duplicate names, unsupported types, bad defaults)
}
```

The source of truth for the exact binding rules is the package itself: [`gomatic/go-flagged`](https://github.com/gomatic/go-flagged).
