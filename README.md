# github.com/typeship-ax/go

Go SDK for the typeship API. [API reference](./api.md)

Resolve an OpenAPI or GraphQL Spec, diagnose it, and keep every selected CLI, MCP, and SDK Target current.

## Installation

```sh
go get github.com/typeship-ax/go@v0.26.0
```

Requires Go 1.21+. The module has no dependencies outside the standard library.

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
	client, err := typeship.New(typeship.WithBearerToken(os.Getenv("TYPESHIP_API_KEY")))
	if err != nil {
		panic(err)
	}

	ctx := context.Background()
	result, err := client.Organization.Get(ctx)
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

- **Bearer token**: `typeship.WithBearerToken` (or `WithBearerTokenFunc` for tokens that expire), sent as `Authorization: Bearer <token>`.

`client.WithCredentials(...)` returns a client with different credentials that shares this client's HTTP client and settings, for per-user or per-tenant calls.

`typeship.WithOnRequest` sees every request before it is sent, for headers every call needs (API version headers, tenant ids).

## Errors

Each status family has one type, returned whether or not the operation
documents the status: `BadRequestError` (400), `UnauthorizedError` (401),
`ForbiddenError` (403), `NotFoundError` (404), `ConflictError` (409),
`UnprocessableEntityError` (422), `RateLimitError` (429), and `ServerError`
(5xx). Match at whichever precision you need:

```go
var notFound *typeship.NotFoundError
if errors.As(err, &notFound) {
	// every 404, documented or not
}

var apiErr *typeship.APIError
if errors.As(err, &apiErr) {
	fmt.Println(apiErr.Code, apiErr.Status, apiErr.RequestID, apiErr.Body, apiErr.Error())
}

var transport *typeship.TransportError
if errors.As(err, &transport) {
	// no response at all: network, DNS, timeout, cancelled context
}
var parseErr *typeship.ResponseParseError
if errors.As(err, &parseErr) {
	fmt.Println(parseErr.Code, parseErr.Status, parseErr.RequestID, parseErr.Body)
}
```

Every error carries `Code`, `Status`, `RequestID`, `Body`, and an actionable `Error()` message. `TransportError.Status` is zero when no HTTP response arrived.

## Runtime validation

Types catch mistakes when you compile; they cannot see an API that has drifted from its spec at runtime. `WithValidation` checks JSON request and response bodies against the spec's own schemas, with no dependencies, since the tables ship as plain data in this package:

```go
client, err := typeship.New(typeship.WithValidation(typeship.ValidateError)) // *ValidationError on mismatch
// or typeship.ValidateWarn, which reports through WithDebug and proceeds
```

A request body is checked before it reaches the wire, so a call that would have been rejected never leaves the process. `ValidationError.Violations` lists each path and what was wrong with it. Off by default: validation costs a walk of every body.

## Response metadata

Pass `WithAPIResponse` to read the status, headers, and request id of a call alongside its payload:

```go
var meta typeship.APIResponse
result, err := client.Organization.Get(ctx, typeship.WithAPIResponse(&meta))
fmt.Println(meta.StatusCode, meta.RequestID, meta.Header.Get("RateLimit-Remaining"))
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

Configuration also reads from the environment (`TYPESHIP_BASE_URL`, `TYPESHIP_API_KEY`).

Timeouts apply to each attempt. By default, the client makes up to two retries for `408`, `429`, `500`, `502`, `503`, and `504`; non-idempotent calls retry only on `429`, when the operation declares an idempotency key, or when explicitly enabled. `Retry-After` takes precedence over exponential backoff.

Generated from the OpenAPI spec by [typeship](https://typeship.dev).
