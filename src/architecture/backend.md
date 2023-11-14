# Backend



# Backend Server and authpage


**Backend framework (Server)**: Python **[FastAPI](https://github.com/tiangolo/fastapi)** server running on **uvicorn** (managed by **[gunicorn](https://github.com/benoitc/gunicorn)** in production), which uses **[uvloop](https://github.com/MagicStack/uvloop)** as its async event loop.

**Frontend framework (authpage)**: **[React](https://reactjs.org/)**, built using [Vite](https://vitejs.dev/) as a multi-page app (MPA) and served statically by FastAPI.

**Persistent database (DB)**: **[PostgreSQL](https://www.postgresql.org/)** relational database.

**In-memory key-value store (KV)**: **[Redis](https://redis.io/)**.

We use the async engine of **[SQLAlchemy](https://github.com/sqlalchemy/sqlalchemy)** (only Core, no ORM, we write SQL manually) as a frontend for **[asyncpg](https://github.com/MagicStack/asyncpg)** for all DB operations. **[Alembic](https://github.com/sqlalchemy/alembic)** is used as a migration tool.

The async component of the **[redis-py](https://github.com/redis/redis-py)** library is used as our KV client.

This is an authorization server, authentication server and web app backend API in one. This model is not recommended for large-scale setups but works well for our purposes. It has been designed in a way that makes the components easy to separate.

Client authentication uses the [OPAQUE protocol](https://datatracker.ietf.org/doc/draft-irtf-cfrg-opaque/) (password-authentication key exchange), which protects against agains pre-computation attacks upon server compromise. This makes passwords extra safe in a way that they never leave the client.

Authorization is performed using [OAuth 2.1](https://datatracker.ietf.org/doc/draft-ietf-oauth-v2-1/), with much of [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html) also implemented to make it appropriate for authenticating users. While not technically required, OAuth tokens are generally in the form of [JSON Web Tokens (JWTs)](https://www.rfc-editor.org/rfc/rfc7519.html) and OpenID Connect does require it, so we use them here. Good 3rd-party resources can be found for [OAuth](https://www.oauth.com/) and [JWTs](https://jwt.io/introduction).

In addition to this, we rely heavily on the following libraries:
* [PyJWT](https://github.com/jpadilla/pyjwt) for signing and parsing JSON web tokens.
* [cryptography](https://github.com/pyca/cryptography) for many cryptographic primitives, primarily for encrypting refresh tokens and handling the keys used for signing the JWTs.
* [pydantic](https://github.com/pydantic/pydantic) for modeling and parsing all data throughout the application.

The backend relies on some basic cryptography (see the [cryptogrpahy section](./backend/crypto.md)). It is nice to know something about secret key cryptography (AES), public key cryptography and hashing.

### Configuration and import structure

The first loaded module is loaded is `define/define.py`. It should be fully independent of any other modules. It determines the so-called "compiled configuration", i.e. variables you want to access easily in code without having to resort to any kind of application state. This means the variables must always be available statically, before the application is started and should be compiled into any deployment. For example, when deployed as a Docker container, these should already be included inside the container. However, they could also vary between "local development" and "production". By default, it loads the `define.toml` in the resources folder. This file is populated with values meant for local development and environment-less testing (i.e. without the database). The file should include a list of environments the variables are applicable to.

Variables that are more "runtime" are loaded in by `env.py`. These are only accessible by loading application state (after startup). Using the `APISERVER_CONFIG` environment variable the path of the config file can be set. By default, it is the incomplete `env.toml` in the resources file. For a development environment, this variable should be set, with `APISERVER_ENV="localdev"` included in the `.toml` file.

To prevent circular imports, no module may depend on a module below itself in this list:

#### TODO UPDATE

### Important to keep in mind

Always add a trailing "/" to endpoints.

### Before you commit

We use [`black`](https://github.com/psf/black) for code formatting and [`ruff`](https://github.com/charliermarsh/ruff) for linting.

```
poetry run black src tests
poetry run ruff src tests
```

## Background info

### Why did we choose \<x\>?

#### FastAPI

FastAPI was selected because of its modern features reliant on Python typing, which greatly simplify development. FastAPI is built on [Starlette](https://github.com/encode/starlette), a lightweight async web server framework. We wanted a lightweight framework that is not too opinionated, as we wanted full control over as many components as possible. Flask would have been another option, but the heavy integration of typing in FastAPI made us choose it instead. Of course, there are also many other options outside the Python ecosystem. We chose to use Python simply because it is very well-known among university students.


#### Redis and PostgreSQL

PostgreSQL and Redis were selected simply by their popularity and open-source status. They have the most libraries built for them, have a large feature set and are widely supported. We chose a relational database because we do not need massive scaling and having relational constraints simplifies keeping all data in sync. For Redis, we use the [RedisJSON](https://github.com/RedisJSON/RedisJSON) extension module to greatly simplify temporarily storing dictionary-like datastructures for storing state across requests. Since there are a great many specific data types that need to be persisted, and they do not have any interdependency, this is much easier to do in an unstructured key-value store like Redis. It is also much faster than having to do this all in a structured, relational database. Note that all DB and KV accesses are heavily abstracted, the underlying queries could easily be re-implemented in other database systems if necessary.

We went all-in on async, expecting database and IO calls to make up the majority of response times. Using async, other waiting requests can be handled in the mean-time.


#### OAuth

Implementing good authentication/authorization for a website is hard. There are many mistakes to be made. However, many available libraries are very opinionated and hard to hook in to. Furthermore, the options become qutie limited when there is approximately no budget. There are some self-hosted solutions, but getting the configuration right can be very tricky and none were found that served our needs. As a result, we went for our own solution, but built using well-regarded web standards to ensure there are no security holes. OAuth is used by every major website nowadays, so the choice was easy. 


#### OPAQUE

OPAQUE is an in-development protocol that seeks to provide a permanent solution to the question of how to best store passwords and authenticate users using them. A simple hash-based solution would have been good enough, but there are many (good and bad) ways to implement this, while OPAQUE makes it much more straightforward to implement it the right way. It also provides tangible security benefits. It has also been used by big companies (for example by WhatsApp for their end-to-end encrypted backups), so it is mature enough for production use.

Our implementation relies on [opaque-ke](https://github.com/novifinancial/opaque-ke), a library written in Rust. As there is no Python library, a simple wrapper for the Rust library, [opquepy](https://github.com/tiptenbrink/opaquebind/tree/main/opaquepy), was written for this project. It exposes the necessary functions for using OPAQUE and consists of very little code, making it easy to maintain. The wrapper is part of a library that also includes a WebAssembly wrapper, which allows it to be called from JavaScript in the browser.

## Maybe implement in future

- https://datatracker.ietf.org/doc/html/rfc8959 secret-token
- https://datatracker.ietf.org/doc/html/rfc7009 token revocation request
- https://auth0.com/docs/secure/tokens/json-web-tokens/json-web-key-sets JSON web key sets
- https://datatracker.ietf.org/doc/html/rfc8414 OAuth 2 discovery
- https://www.rfc-editor.org/rfc/rfc9068 Access token standard (also proper OpenID scope)
- https://datatracker.ietf.org/doc/html/rfc7662 token metadata (introspection)