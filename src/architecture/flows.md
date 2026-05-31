# Flows

This page summarizes the important account and member flows. For the full backend contract, use `dodeka/backend/docs/spec.md`.

## Registration

1. A user requests registration on the website.
2. The Python backend creates or reuses a pending `registration_id`.
3. An admin accepts the registration manually or through a Volta sync import.
4. The backend creates a Faroe signup and sends an invite email.
5. The user verifies email, sets a password, and Faroe creates the account.
6. The frontend sends the Faroe session token to the backend, which sets the session cookie.

A user only gets member-only access after the backend has a live user with the `member` permission.

## Login

Login is handled by Faroe. The frontend creates and completes a Faroe signin, then sends the session token to the Python backend. The backend validates it and stores it in an HTTP-only cookie.

## Permissions

Permissions live in the backend `users` table. `member` grants member pages and `admin` grants the admin panel. Committee roles are stored as extra permission tags. Permissions expire after one year unless renewed.

## Volta Sync

Admins import a VoltaClub CSV. The backend links rows by `bondsnummer`, detects new/current/departed members, and asks for admin review when it cannot safely match data automatically.

Sync can:

- accept new members and send signup invites
- renew current members' `member` permission
- revoke `member` for departed users
- update linked user or registration data from Volta

## Email

Auth and registration emails are sent by the backend. Some emails are written to an internal outbox first, so they can still be retried after the database change has committed.
