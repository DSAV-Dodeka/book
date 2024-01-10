# Changelog

Hier houden we alle veranderingen bij. Dit gaat specifiek om code, niet om content. De frontend wordt vaker geüpdate dan hier weergegeven (en heeft ook geen specifieke versies), maar de belangrijke veranderingen zijn wel hier samengevat.

<!-- toc -->

## 2.2.0 - 2024-01-10

This is the first full `tidploy` release on the backend with the mailserver moved to a dedicated provider. This should make all required features for the classifications fully work.

Senne en Tip hebben code aan deze release bijgedragen. We are now at 591 commits for the backend (including the 150+ commits that were from the deployment repository).

### Added (backend)

* `<server>/admin/class/get_meta/{recent_number}/` (this returns the most recent classifications of each type, recent_number/2 for each type)
* `<server>/admin/class/new/` (creates a new points and training classification)
* `<server>/admin/class/modify/` (modifies a classification)
* `<server>/admin/class/remove/{class_id}` (removes classification with id class_id)
* `<server>/member/get_with_info/{rank_type}/` (Will replace `<server>/members/class/get/{rank_type}/`, but added with different name to avoid breaking change, this also returns last_updated and whether the classification is frozen)
* The last_updated column is now updated when new events are added. 
* (Senne) `<server>/admin/update/training` has been added (not yet in use by frontend) as the start of the training registration update.
* New `update_by_unique` store method for updating an entire row based on a simple where condition.

### Internal (backend)

* The backend and deployment repository have been merged together, making deployment and versioning a lot easier. We now use a bunch of Nu scripts instead of shell scripts and the deployment will now use [`tidploy`](https://github.com/tiptenbrink/tidploy). This done in a series of commits from [4602abd](https://github.com/DSAV-Dodeka/dodeka/commit/4602abd986e67c39c2077215ac7310115cac9aaf) to around [414bc23](https://github.com/DSAV-Dodeka/dodeka/commit/414bc23ccc478f17be90169ed4dd16852a3df6dd).
* Logging has been much improved throughout the backend, so errors can now be properly seen. Error handling has also been improved (especially for the store module). Some common startup errors now have better messages that indicate what's wrong. Documentation has been improved and all database queries now properly catch some specific errors. See [#31](https://github.com/DSAV-Dodeka/dodeka/pull/31).

## 2.1.1 - 2023-12-05

Tip heeft code aan deze release code bijgedragen.

### Fixed (backend)
* Remove debug eduinstitution value during registration

## 2.1.0 - 2023-12-05

Note: this version was not released into production.

Matthijs en Tip hebben code aan deze release bijgedragen.

### Stats

We are now at 359 commits on the backend and 922 commits on the frontend. 

### Added (frontend)
- An explanation on the classification has been added to the classification page.
- There is now an NSK Meerkamp role.

### Fixed (frontend)
- Last names are no longer all caps on the classification page and are shown in full.

### Added (authpage)
* Add Dodeka logo to all pages, using new `Title` component.

### Changed (authpage)
* Use flex layout to make alignment better when pressing "forgot password", so no the layout doesn't jump slightly
* Update node version to v20
* Update dependencies

### Fixed (authpage)
* Educational institution is now recorded if not changed from default (TU Delft)

### Added (backend)
- **Admin**: Synchronization of the total points per user and events, as well as a more consistent naming scheme for the endpoints. All old endpoints are retained for backwards compatibility. Furthermore, admins can now request additional information about events on a user, event or class basis (see [the PR](https://github.com/DSAV-Dodeka/backend/pull/66)).
    - `<server>/admin/class/sync/` (Force synchronization of the total points per user, without having to add a new event)
    - `<server>/admin/class/update/` (Same as previous `<server>/admin/ranking/update`, which still exists for backwards compatibility)
    - `<server>/admin/class/get/{rank_type}/` (Same as previous `<server>/admin/classificaiton/{rank_type}/`, which still exists for backwards compatibility)
    - `<server>/admin/class/events/user/{user_id}/` (Get all events for a specific user_id, with the class_id and rank_type as query parameters)
    - `<server>/admin/class/events/all//` (Get all events, with the specific class_id and rank_type as query parameters)
    - `<server>/admin/class/users/event/{event_id}/` (Get all users for a specific event_id)
- **Member**: Only renames, as described above.
    - `<server>/members/class/get/{rank_type}/` (Same as previous `<server>/members/classificaiton/{rank_type}/`, which still exists for backwards compatibility)
    - `<server>/members/profile/` (Same as previous `<server>/res/profile`, which still exists for backwards compatibility)

### Changed (backend)
- **Types**: The entire backend now passes mypy's type checker (see [the PR](https://github.com/DSAV-Dodeka/backend/pull/68))!
- **Better context/dependency injection**: The previous system was not perfect and it was still not easy to write tests. Lots of improvements have been made, utilizing FastAPI Depends and making it possible to easily wrap a single function call to make the caller testable. See [#64](https://github.com/DSAV-Dodeka/backend/pull/64), [#65](https://github.com/DSAV-Dodeka/backend/pull/64), [#70](https://github.com/DSAV-Dodeka/backend/pull/70) and [#71](https://github.com/DSAV-Dodeka/backend/pull/71).
- **Better logging**: Logging had been lackluster while waiting for a better solution. This has now arrived with the adoption of loguru. Logging is now much more nicely formatted and it will be easily possible in the future to collect and show the logs in a central place, although that is not yet implemented. Some of the startup code has also been refactored as part of the logging effort. 
- **Check for role on router basis**: For certain routers, we now check whether they are requested by admins or members for all routes inside the router, making it harder to forget to add a check. The header checking logic has also been refactored and some tests have been added. Much better than the manual `if` check we did before. This also includes some minor refactor and fixes for access token verification.
- There are now different router tags, which makes it easier to find all the different API endpoints in the OpenAPI docs view.

### Fixed (backend)
- An error is no longer thrown on the backend when a password reset is requested for a user that does not exist.

### Internal (backend)

- **Live query tests**: in the GitHub Actions CI we now actually run some tests against a live database using Actions service containers. This means we can be much more sure that we did not completely break database functionality after passing the tests. [PR](https://github.com/DSAV-Dodeka/backend/pull/69)
- Add request_id to logger using loguru's contextualize
- Added logging to all major user flows (signup, onboard, change email/password), also allowing the display of reset URL's etc. so email doesn't have to be turned on during local development

## 2.0.1 - 2023-10-25

Tip heeft code aan deze release bijgedragen.

Released into production on 2023-10-25.

### Fixed (backend)

- **Fix update email**: If you requested an email change twice, but only confirmed this after they were both sent, it is no longer to change it twice. After changing it using either one, the other one is invalidated.
- Changed package structure so it is possible to extract the schema package and load it on the production server to run database schema migrations.

## 2.0.0 - 2023-10-17

Note: this version was not released into production.

Leander, Matthijs en Tip hebben code aan deze release bijgedragen.

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


## Pre-1.0.0

The frontend went live in June 2021 and before the release of the backend, was regularly updated using a rolling release schedule. The frontend is not versioned.