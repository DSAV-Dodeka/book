# tiauth-faroe

The backend auth server is a Go binary that uses the [tiauth-faroe](https://github.com/tiptenbrink/tiauth-faroe) library. The source lives in `backend/auth/` and the dependency is declared in `backend/auth/go.mod`:

```
require github.com/tiptenbrink/tiauth-faroe/tiauth v0.1.1
```

For now, `tiauth-faroe` is managed by Tip. Ask him if you want something updated or if the CI is broken. Feel free to replace it with something else, there are many other solutions to authentication.

## Running in development

When you run `uv run dev` from the `backend/` directory, the auth server is started automatically alongside the Python backend. If the auth binary is not present, it will be downloaded from GitHub Actions artifacts (requires the `gh` CLI).

You can also run the auth server standalone:

```shell
cd backend/auth
go run . --env-file envs/test/.env
```

The specific version of the auth binary is set in `dodeka/backend/auth/auth-binary.sum`, using SHA256 hashes of the executables in the GitHub release executables for each platform. When a new release for the auth binaries are made, you can automatically update `auth-binary.sum` using `uv run update-auth`. Whenever you run `uv run dev` it checks whether the local binary still has the same hash as described in `auth-binary.sum`. If you compiled a version locally and want to use that, you can use `--local-auth` as a CLI flag. 

## Updating the dependency in dodeka

After a new tiauth-faroe version is tagged and pushed:

```shell
cd backend/auth
go get github.com/tiptenbrink/tiauth-faroe/tiauth@v0.2.0
go mod tidy
```

`go get` updates `go.mod` to the new version. `go mod tidy` cleans up `go.sum` by removing checksums for old versions that are no longer needed. Without `go mod tidy`, `go.sum` retains hashes of all previously resolved versions (this is a security feature, not a bug).

## Building

```shell
cd backend/auth
go build -o auth .
```

CI builds for Linux (x64), macOS (Apple Silicon), and Windows (x64) are produced automatically by GitHub Actions when files in `backend/auth/` change. See `.github/workflows/ci.yml`.
