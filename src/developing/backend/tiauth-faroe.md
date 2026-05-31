# tiauth-faroe

The backend auth server is a Go binary that uses the [tiauth-faroe](https://github.com/tiptenbrink/tiauth-faroe) library.

The source lives in `backend/auth/`. Check `backend/auth/go.mod` for the current Go and `tiauth-faroe` versions; do not copy a version from this book page.

For now, `tiauth-faroe` is managed by Tip. Ask him if you want something updated or if the CI is broken. Feel free to replace it with something else, there are many other solutions to authentication. Just make sure you know what you are doing!

## Running in development

When you run `uv run dev` from the `backend/` directory, the auth server is started automatically alongside the Python backend. If the auth binary is missing or does not match `backend/auth/auth-binary.sum`, it is downloaded from the pinned GitHub release asset using the `gh` CLI. So everything should work out of the box and should not require any additional work!

### Running auth server standalone (expert)

You can also run the auth server standalone, but this requires installing the Go programming language:

```shell
cd backend/auth
go run . --env-file envs/test/.env
```

The pinned binary version is set in `dodeka/backend/auth/auth-binary.sum`, using SHA256 hashes of release assets for each platform. See `dodeka/backend/README.md` for the current release and `update-auth` commands. If you compiled a binary locally and want to use it, pass `--local-auth` to `uv run dev`.

## Updating the dependency in dodeka (expert)

After a new tiauth-faroe version is tagged and pushed:

```shell
cd backend/auth
go get github.com/tiptenbrink/tiauth-faroe/tiauth@<version>
go mod tidy
```

`go get` updates `go.mod` to the new version. `go mod tidy` cleans up `go.sum` by removing checksums for old versions that are no longer needed. Without `go mod tidy`, `go.sum` retains hashes of all previously resolved versions (this is a security feature, not a bug).

## Building (expert)

```shell
cd backend/auth
go build -o auth .
```

CI builds are in `.github/workflows/auth-ci.yml`. Release binaries are built and attached by `.github/workflows/auth-release.yml` when an `auth/v*` tag is pushed.
