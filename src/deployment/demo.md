## Running the auth server

```
CGO_ENABLED=1 go run . --insecure --env-file .env.demo --interactive --no-smtp
```

## Running the backend

```
uv run --locked --no-dev backend
```
