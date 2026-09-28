# Publishing github.com/typeship-ax/go

How to build, release, and maintain this package. Its users need only [README.md](README.md).

## Build and test

Requires Go 1.21+. From this directory:

```sh
go build ./...
go vet ./...
```

## Publish the module

`go.mod` declares `github.com/typeship-ax/go`. Push a `v<major>.<minor>.<patch>` tag for each release; users then run `go get github.com/typeship-ax/go@<version>`. Moving to a new major version (v2 or later) also changes the module path's `/vN` suffix, as Go requires.

## Customizing this package

- A custom file ships only when the package manifest, exports, build, and tests include it. Add a package check for every custom build or test step.
- Keep application-only wrappers outside this package. Code shipped from this package must pass the package's checks.
- When this package's repository receives reviewed regeneration pull requests, committed customizations are preserved and edits that overlap a generated change stop for review. Regenerating into a directory replaces its files.
