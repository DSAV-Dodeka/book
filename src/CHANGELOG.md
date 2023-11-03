# Changelog

Hier houden we alle veranderingen bij. Dit gaat specifiek om code, niet om content.

## 2.0.1 - 2023-10-25

Released into production on 2023-10-25.

### Fixed (backend)

- **Fix update email**: If you requested an email change twice, but only confirmed this after they were both sent, it is no longer to change it twice. After changing it using either one, the other one is invalidated.
- Changed package structure so it is possible to extract the schema package and load it on the production server to run database schema migrations.

## 2.0.0 - 2023-10-17

Note: this version was not released into production.

### Added (backend)
- **Admin**: Roles, using OAuth scope mechanism, as well as classifications stored in the database, computed based on each event.
    - `<server>/admin/scopes/all/` (Get scopes for all users)
    - `<server>/admin/scopes/add/` (Add scope for a user)
    - `<server>/admin/scopes/remove/` (Remove scope for a user)
    - `<server>/admin/users/ids/` (Get all user ids)
    - `<server>/admin/users/names/` (Get all user names, to match for rankings)
    - `<server>/admin/ranking/update` (Add an event for classifications)
    - `<server>/admin/classification/{rank_type}/` (See current points for a specific classification)
- **Member**:
    - `<server>/members/classification/{rank_type}/` (See current points for a specific classification, changes hidden after certain point)

### Added (frontend)

- **Member -> Classification page**
- **Admin -> Classification page** and **Add new event**
- Roles can be changed in user overview

### Changed (backend)

- Major refactor of backend code, which separates auth code from app-specific code
- Updated some major dependencies, including Pydantic to v2
- **Database schema update**: 
    - `classifications` table added to store a classification, which lasts for half a year and can be either "points" or "training".
    - `class_events` table added, which stores all events that have been held (borrel, NSK, training, ...). Possibly related to a specific classification.
    - `class_event_points` table added, which stores how many points a specific user has received for a specific event. In general, users will have the same amount of points per event, but this flexibility allows us to change that later.
    - `class_points` table added, which stores the aggregated total points of a user for a specific classification. When an event is added, this table should be updated using the correct update query.

### Changed (postgres)

- Updated from Postgres 14 to 16

### Changed (redis)

- Updated from Redis 6.2 to 7.2

## 1.1.0 - 2023-3-18

Released into production.

### Changed (backend)

- Update dependencies, including updating Python to 3.11 and SQLAlchemy 2

### Fixed (server)

- Docker conatiner no longer accumulates core dumps, crashing the server after 1-2 weeks

## 1.0.0 - 2023-1-11

Initial release of the FastAPI backend server, PostgreSQL database and Redis key-value store. Released into production on 2023-1-11.

### Added (backend)

- **Login**: Mostly OAuth 2.1-compliant authentication/authorization system, using the authorization code flow. Authentication is done using OPAQUE:
    - `<server>/oauth/authorize/` (Authorization Endpoint initialize)
    - `<server>/oauth/callback/` (Authorization Endpoint after authentication)
    - `<server>/oauth/token/` (Token Endpoint)
    - `<server>/login/start/` (Start OPAQUE password authentication)
    - `<server>/login/finish/` (Finish OPAQUE authentication)
- **Registration**: Registration/onboarding flow, which requires confirmation of AV`40 signup.
    - `<server>/onboard/signup/` (Initiate signup, board will confirm)
    - `<server>/onboard/email/` (Confirm email)
    - `<server>/onboard/confirm/` (Board confirms signup)
    - `<server>/onboard/register/` (Start OPAQUE registration)
    - `<server>/onboard/finish/` (Finish OPAQUE registration and send registration info)
- **Update**: Some information needs to be updated or changed.
    - `<server>/update/password/reset/` (Initiate password reset)
    - `<server>/update/password/start/` (Start OPAQUE set new password)
    - `<server>/update/password/finish/` (Finsih OPAQUE set new password)
    - `<server>/update/email/send/` (Initiate email change)
    - `<server>/update/email/check/` (Finish email change after authentication)
    - `<server>/update/delete/url/` (Delete account start, create url)
    - `<server>/update/delete/check/` (Confirm deletion after authentication)
- **Admin**: Get information only with admin account.
    - `<server>/admin/users/` (Get all user data)
- **Members**: Get information for members.
    - `<server>/members/birthdays/` (Get member birthdays)

### Added (authpage)

- **Login page**
- **Registration page**
- Other confirmation pages necessary for backend functionality

### Added (frontend)

- **Profile page**
- **Leden page -> Verjaardagen**
- **Admin page -> Ledenoverzicht**
- Use React Query for getting data from backend
- Use Context from React for authentication state
- Redirect pages necessary for OAuth
- Confirmation pages for info update

### Added (server)

- Docker container that contains the FastAPI backend server, as well as static files for serving the authpage.

### Added (postgres)

- Docker conatiner with PostgreSQL server

### Added (redis)

- Docker container with Redis server