# typeship: agent guide

Instructions for coding agents that call the typeship API through this Go SDK (API version 1.0.0, package version 0.26.0).

Resolve an OpenAPI or GraphQL Spec, diagnose it, and keep every
selected CLI, MCP, and SDK Target current.

Every operation but one requires a bearer credential: an organization
API key from the console, or an OAuth access token carrying the operation's
read, generate, or write capability and the organization selected during
consent. OAuth grants cannot switch organizations after consent. A browser
session is not a credential for this API. The exception is POST /generate,
which works anonymously with the free plan's limits.

Examples use Parcel, a fictional delivery service. Replace its domains,
repository names, and resource identifiers with your own. The hosted
petstore Spec is a runnable sample.

## Before writing code
- `api.md` is the method reference; `api.json` is the machine-readable contract: every operation's inputs, outputs, errors, `safety` (`read`, `write`, or `destructive`), and an example. Look up exact names there instead of guessing.
- `README.md` covers installation and setup.
- Zero dependencies: `go.mod` has no requires, so nothing third-party enters your dependency graph.

## Authentication
- Bearer token: `TYPESHIP_API_KEY` env var, or the `WithBearerToken` option.

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

## Safety
- Read credentials from the environment or a secret store. Never hard-code them, print them, or put them in URLs or command arguments.
- Check an operation's `safety` in `api.json` before calling it. Confirm with the user before running a `write` or `destructive` operation they did not ask for.
- The client already retries transient failures, honoring `Retry-After`, and retries a write only when that is safe. Do not wrap calls in another retry loop: a repeated write can apply twice.

## Documentation
- The reference for this exact package: `api.md` (offline, always current with the code).
- Conceptual guides live on the docs site. For questions about how the API's concepts fit together (flows, ordering, environments), fetch `https://typeship.dev/llms-full.txt` and read the relevant sections; `https://typeship.dev/llms.txt` is the page index. Relative links in the spec resolve against `https://typeship.dev`.
