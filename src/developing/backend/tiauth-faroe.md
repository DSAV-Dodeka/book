# tiauth-faroe

The backend auth server is a Go binary that uses the [tiauth-faroe](https://github.com/tiptenbrink/tiauth-faroe) library. The source lives in `backend/auth/` and the dependency is declared in `backend/auth/go.mod`:

```
require github.com/tiptenbrink/tiauth-faroe/tiauth v0.1.1
```

## Running in development

When you run `uv run dev` from the `backend/` directory, the auth server is started automatically alongside the Python backend. If the auth binary is not present, it will be downloaded from GitHub Actions artifacts (requires the `gh` CLI).

You can also run the auth server standalone:

```shell
cd backend/auth
CGO_ENABLED=1 go run . --env-file envs/test/.env --enable-reset --interactive
```

To force-download or update the pre-built binary:

```shell
cd backend
uv run update-auth
```

## Releasing a new tiauth-faroe version

The tiauth-faroe Go module lives in a subdirectory (`tiauth/`) of the tiauth-faroe repository. Go modules in subdirectories require git tags prefixed with the subdirectory path.

To release a new version:

```shell
# In the tiauth-faroe repository
git tag -a tiauth/v0.2.0 -m "description of changes"
git push origin tiauth/v0.2.0
```

The Go module proxy (`proxy.golang.org`) picks up the tag automatically from GitHub. No CI or publish step is needed.

## Updating the dependency in dodeka

After a new tiauth-faroe version is tagged and pushed:

```shell
cd backend/auth
go get github.com/tiptenbrink/tiauth-faroe/tiauth@v0.2.0
go mod tidy
```

`go get` updates `go.mod` to the new version. `go mod tidy` cleans up `go.sum` by removing checksums for old versions that are no longer needed. Without `go mod tidy`, `go.sum` retains hashes of all previously resolved versions (this is a security feature, not a bug).

## Building

Building requires CGO because of the [mattn/go-sqlite3](https://github.com/mattn/go-sqlite3) dependency:

```shell
cd backend/auth
CGO_ENABLED=1 go build -o auth .
```

CI builds for Linux (x64), macOS (Apple Silicon), and Windows (x64) are produced automatically by GitHub Actions when files in `backend/auth/` change. See `.github/workflows/ci.yml`.
