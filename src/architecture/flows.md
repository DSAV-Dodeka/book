# Flows

This mostly describes the important user "flows" and state that is captured for registration, authentication and related. They are mostly built around [faroe](), which we use for the actual password checking, handling, e-mail codes, registration and more. However, we layer some additional flows on top of it and save additional state because our source of truth is Volta and admins must accept that users really signed up there before they can get access to the website.

## Registration

Registration is the most important to understand. First of all, a user is only fully registered when it has an "account" (has been registered through the Faroe flow, which mean it has an entry in the `users` and `users_by_email` table) and when it has "member" permissions, also stored in the `users` table.

Creating an account can happen fully user-initiated through the website's "Word lid" page, but admin approval is required to give "member" permissions. These permissions can be given in two ways:
- An admin manually approves a user
- An admin imports a file from VoltaClub containing all active users and uploads it to the backend, through which it can then batch accept all users

The latter flow also
