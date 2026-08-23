# github.com/typeship-ax/go

Go SDK for typeship. [API reference](./api.md)

Generated from the OpenAPI spec by [typeship](https://typeship.dev). Change the spec or generation settings, then regenerate; generated files are not hand-edited.

- **Zero dependencies** — `go.mod` has no requires; the client uses only the standard library
- **Typed errors** — every documented error response is a type you can match with `errors.As`
- **Context-aware** — every call takes a `context.Context`, so cancellation and deadlines work
- **Retries built in** — idempotent requests retry with exponential backoff and `Retry-After` support

## Install

```sh
go get github.com/typeship-ax/go
```

## Quickstart

```go
package main

import (
	"context"
	"errors"
	"fmt"
	"os"

	"github.com/typeship-ax/go"
)

func main() {
	client, err := typeship.New(typeship.WithBearerToken(os.Getenv("TYPESHIP_TOKEN")))
	if err != nil {
		panic(err)
	}

	ctx := context.Background()
	result, err := client.Projects.Retrieve(ctx, "id_123")
	if err != nil {
		var apiErr *typeship.APIError
		if errors.As(err, &apiErr) {
			fmt.Println(apiErr.Status, apiErr.RequestID)
		}
		panic(err)
	}
	fmt.Println(result)

	it := client.Projects.List(ctx, nil)
	for it.Next() {
		fmt.Println(it.Value())
	}
	if err := it.Err(); err != nil {
		panic(err)
	}
}
```

## Authentication

- **Bearer token** — `typeship.WithBearerToken` (or `WithBearerTokenFunc` for tokens that expire), sent as `Authorization: Bearer <token>`.

`typeship.WithOnRequest` sees every request before it is sent, for headers every call needs (API version headers, tenant ids).

## Errors

Every documented error response has its own type, so you can match at
whichever precision you need:

```go
var badRequest *typeship.BadRequestError
if errors.As(err, &badRequest) {
	// handle the documented 400
}

var apiErr *typeship.APIError
if errors.As(err, &apiErr) {
	fmt.Println(apiErr.Status, apiErr.RequestID) // any error response, documented or not
}

var transport *typeship.TransportError
if errors.As(err, &transport) {
	// no response at all: network, DNS, timeout, cancelled context
}
```

## Runtime validation

Types catch mistakes when you compile; they cannot see an API that has drifted from its spec at runtime. `WithValidation` checks JSON request and response bodies against the spec's own schemas — no dependencies, since the tables ship as plain data in this package:

```go
client, err := typeship.New(typeship.WithValidation(typeship.ValidateError)) // *ValidationError on mismatch
client, err := typeship.New(typeship.WithValidation(typeship.ValidateWarn))  // reports via WithDebug, proceeds
```

A request body is checked before it reaches the wire, so a call that would have been rejected never leaves the process. `ValidationError.Violations` lists each path and what was wrong with it. Off by default: validation costs a walk of every body.

## Response metadata

Pass `WithAPIResponse` to read the status, headers, and request id of a call alongside its payload:

```go
var meta typeship.APIResponse
result, err := client.Projects.Retrieve(ctx, "id_123", typeship.WithAPIResponse(&meta))
fmt.Println(meta.StatusCode, meta.RequestID, meta.Header.Get("X-RateLimit-Remaining"))
```

## Configuration

```go
client, err := typeship.New(
	typeship.WithBaseURL("https://typeship.dev/api/v1"), // overrides the default
	typeship.WithTimeout(10*time.Second),   // per attempt
	typeship.WithMaxRetries(3),
	typeship.WithHTTPClient(myClient),      // proxies, transports, tracing
	typeship.WithDebug(func(e typeship.DebugEvent) { log.Println(e.Method, e.Path, e.Status) }),
)
```

Configuration also reads from the environment (`TYPESHIP_BASE_URL`, `TYPESHIP_TOKEN`).
