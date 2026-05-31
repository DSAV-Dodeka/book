# Frontend vs Backend

The frontend is the website code that runs in the browser. It owns pages, styling, navigation, forms, and calls to the backend.

The backend runs on the server. It owns private state: users, registrations, permissions, imported Volta data, session validation, and member-only data.

The auth server is a separate Go process. It owns signup, login, password flows, email verification codes, and session tokens. The Python backend validates those sessions and maps them to Dodeka permissions.

The frontend talks to:

- the Python backend through wrappers in `dodekafrontend/src/functions/backend.ts`
- Faroe auth through `dodekafrontend/src/functions/faroe.ts`

The backend exposes public API routes from `dodeka/backend/src/apiserver/app.py`. It also has a private loopback API for the auth server and backend CLI tools.

Keep the split simple:

- public content belongs in the frontend repository
- private or member-only data belongs in the backend database
- passwords and auth protocol belong in Faroe
- exact backend rules belong in `dodeka/backend/spec.md`
