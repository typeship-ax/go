# typeship — agent context

This package contains the generated Go SDK for **typeship** (API v1.0.0, package v1.0.0).

Resolve an OpenAPI or GraphQL Definition, diagnose it, and keep every
selected SDK, CLI, and MCP Target current.

Every operation but one requires a bearer credential: an organization
API key from the console, or an OAuth access token carrying the operation's
read, generate, or write capability and the organization selected during
consent. OAuth grants cannot switch organizations after consent. A browser
session is not a credential for this API. The exception is POST /generate,
which works anonymously with the free plan's limits.

## Ground rules
- Generated code: never edit files in this package by hand — changes are lost on regeneration. Wrap the client in your own code instead.
- Zero dependencies: `go.mod` has no requires, so nothing third-party enters your dependency graph.
- `api.md` is the native method reference; `api.json` is the machine-readable operation, schema, safety, and example contract. Read them before guessing.

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
- `WithValidation(ValidateError)` checks request and response bodies against the generated schema table; `ValidateWarn` reports drift and continues.
- A `$ref`, array, or text body is a positional `body` argument; inline object bodies are fields of the params struct. Uploads are `Upload{Name, ContentType, Reader}` fields. `oneOf`/`anyOf` values are union types with `As<Variant>()`/`From<Variant>()` accessors and `Discriminator()`.

## Documentation
- The reference for this exact package: `api.md` (offline, always current with the code).
- Conceptual guides live on the docs site. For questions about how the API's concepts fit together (flows, ordering, environments), fetch `https://typeship.dev/llms-full.txt` and read the relevant sections; `https://typeship.dev/llms.txt` is the page index.
