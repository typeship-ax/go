# typeship — agent context

This package is the generated Go SDK for **typeship** (v1.0.0).

Generate a zero-dependency SDK — TypeScript, Python, or Go — plus a CLI
and an MCP server, from an OpenAPI spec.

Every operation but one requires an API key, created in the console and
sent as `Authorization: Bearer ak_...`. A browser session is not a
credential for this API. The exception is POST /generate, which works
anonymously with the free plan's limits.

## Ground rules
- Generated code: never edit files in this package by hand — changes are lost on regeneration. Wrap the client in your own code instead.
- Zero dependencies: `go.mod` has no requires, so nothing third-party enters your dependency graph.
- `api.md` in this package is the complete method reference: every operation, parameter, and error class. Read it before guessing.

## Authentication
- Bearer token: `TYPESHIP_TOKEN` env var, or the `WithBearerToken` option.

## Using the SDK
```go
client, err := typeship.New()  // auth options above
if err != nil {
	return err
}
```
- Every method takes a `context.Context` first, so cancellation and deadlines work the way they do everywhere else in Go.
- Errors are typed: `errors.As(err, &notFound)` for a documented status, `*APIError` for any API error, `*TransportError` for no response at all.
- Optional fields are pointers; slices and maps are already nil-able and stay bare.
- Paginated methods return an `*Iter[T]`: `for it.Next() { it.Value() }`, then check `it.Err()`.
- Every method takes variadic `RequestOption`s (`WithRequestTimeout`, `WithRequestMaxRetries`, `WithRequestHeader`, `WithAPIResponse` for status/headers/request id) for per-call overrides.
- A `$ref`, array, or text body is a positional `body` argument; inline object bodies are fields of the params struct. Uploads are `Upload{Name, ContentType, Reader}` fields. `oneOf`/`anyOf` values are union types with `As<Variant>()`/`From<Variant>()` accessors and `Discriminator()`.

## Documentation
- The reference for this exact package: `api.md` (offline, always current with the code).
- No docs site is configured for this API; the spec-derived reference above is the source of truth.
