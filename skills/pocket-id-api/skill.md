# Pocket ID API

Use this skill when users need to work with the Pocket ID API.

## Required secret

- `POCKET_ID_API_KEY` (provided via environment variable)

## Instructions

1. Read the API description shared by the user and follow its endpoint, method, and payload definitions exactly.
2. Authenticate all API requests with the injected key from `POCKET_ID_API_KEY`.
3. Never hardcode API keys, tokens, or credentials in code, examples, or logs.
4. Prefer concise, production-safe request examples (for example with `curl`) that use environment variables.

## Example authentication pattern

```bash
curl -H "X-API-Key: $POCKET_ID_API_KEY" "<POCKET_ID_API_ENDPOINT>"
```

## API Endpoints

All paths are relative to your Pocket ID base URL. Endpoints marked **(admin)** require an admin account; endpoints marked **(auth)** require any authenticated user. Unannotated endpoints are public.

### Well-Known / OIDC Discovery

| Method | Path | Description |
|--------|------|-------------|
| GET | `/.well-known/openid-configuration` | OpenID Connect discovery document |
| GET | `/.well-known/jwks.json` | JSON Web Key Set (JWKS) |

### Authentication / WebAuthn

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/webauthn/login/start` | Begin passkey login |
| POST | `/api/webauthn/login/finish` | Complete passkey login |
| POST | `/api/webauthn/logout` | **(auth)** Log out |
| POST | `/api/webauthn/reauthenticate` | **(auth)** Reauthenticate |
| GET | `/api/webauthn/register/start` | **(auth)** Begin passkey registration |
| POST | `/api/webauthn/register/finish` | **(auth)** Complete passkey registration |
| GET | `/api/webauthn/credentials` | **(auth)** List own WebAuthn credentials |
| PATCH | `/api/webauthn/credentials/:id` | **(auth)** Rename a WebAuthn credential |
| DELETE | `/api/webauthn/credentials/:id` | **(auth)** Delete a WebAuthn credential |

### Signup

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/signup/setup` | Check if initial admin setup is available |
| POST | `/api/signup/setup` | Sign up the initial admin user |
| POST | `/api/signup` | Sign up with a signup token |
| POST | `/api/signup-tokens` | **(admin)** Create a signup token |
| GET | `/api/signup-tokens` | **(admin)** List signup tokens |
| DELETE | `/api/signup-tokens/:id` | **(admin)** Delete a signup token |

### One-Time Access

| Method | Path | Description |
|--------|------|-------------|
| POST | `/api/one-time-access-email` | Request a one-time access email (unauthenticated) |
| POST | `/api/one-time-access-token/:token` | Exchange a one-time access token for a session |
| POST | `/api/users/me/one-time-access-token` | **(auth)** Create own one-time access token |
| POST | `/api/users/:id/one-time-access-token` | **(admin)** Create one-time access token for a user |
| POST | `/api/users/:id/one-time-access-email` | **(admin)** Send one-time access email to a user |

### Users

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/users/me` | **(auth)** Get current user |
| PUT | `/api/users/me` | **(auth)** Update current user |
| GET | `/api/users/me/profile-picture.png` | **(auth)** Get current user's profile picture |
| PUT | `/api/users/me/profile-picture` | **(auth)** Update current user's profile picture |
| DELETE | `/api/users/me/profile-picture` | **(auth)** Reset current user's profile picture |
| POST | `/api/users/me/send-email-verification` | **(auth)** Send email verification |
| POST | `/api/users/me/verify-email` | **(auth)** Verify email address |
| GET | `/api/users` | **(admin)** List users |
| POST | `/api/users` | **(admin)** Create a user |
| GET | `/api/users/:id` | **(admin)** Get user by ID |
| PUT | `/api/users/:id` | **(admin)** Update user |
| DELETE | `/api/users/:id` | **(admin)** Delete user |
| GET | `/api/users/:id/groups` | **(admin)** Get groups of a user |
| PUT | `/api/users/:id/user-groups` | **(admin)** Set groups for a user |
| GET | `/api/users/:id/profile-picture.png` | Get user profile picture |
| PUT | `/api/users/:id/profile-picture` | **(admin)** Update user profile picture |
| DELETE | `/api/users/:id/profile-picture` | **(admin)** Reset user profile picture |
| GET | `/api/users/:id/webauthn-credentials` | **(admin)** List WebAuthn credentials for a user |
| DELETE | `/api/users/:id/webauthn-credentials/:credentialId` | **(admin)** Delete a user's WebAuthn credential |

### User Groups

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/user-groups` | **(admin)** List user groups |
| POST | `/api/user-groups` | **(admin)** Create a user group |
| GET | `/api/user-groups/:id` | **(admin)** Get user group by ID |
| PUT | `/api/user-groups/:id` | **(admin)** Update a user group |
| DELETE | `/api/user-groups/:id` | **(admin)** Delete a user group |
| PUT | `/api/user-groups/:id/users` | **(admin)** Set members of a user group |
| PUT | `/api/user-groups/:id/allowed-oidc-clients` | **(admin)** Set allowed OIDC clients for a group |

### OIDC

| Method | Path | Description |
|--------|------|-------------|
| POST | `/api/oidc/token` | Token endpoint (authorization code, refresh token, device code, client credentials) |
| POST | `/api/oidc/par` | Pushed Authorization Request (PAR) |
| GET | `/api/oidc/userinfo` | UserInfo endpoint |
| POST | `/api/oidc/userinfo` | UserInfo endpoint (POST) |
| POST | `/api/oidc/introspect` | Token introspection |
| POST | `/api/oidc/device/authorize` | Device authorization request |
| POST | `/api/oidc/authorize` | **(auth)** Authorize an OIDC request |
| POST | `/api/oidc/authorization-required` | **(auth)** Check if authorization confirmation is needed |
| POST | `/api/oidc/end-session` | **(auth)** End session / logout |
| GET | `/api/oidc/end-session` | **(auth)** End session / logout |
| POST | `/api/oidc/device/verify` | **(auth)** Verify a device code |
| GET | `/api/oidc/device/info` | **(auth)** Get device code information |
| GET | `/api/oidc/users/me/authorized-clients` | **(auth)** List own authorized OIDC clients |
| DELETE | `/api/oidc/users/me/authorized-clients/:clientId` | **(auth)** Revoke own authorization for an OIDC client |
| GET | `/api/oidc/users/me/clients` | **(auth)** List OIDC clients accessible to current user |
| GET | `/api/oidc/clients` | **(admin)** List OIDC clients |
| POST | `/api/oidc/clients` | **(admin)** Create an OIDC client |
| GET | `/api/oidc/clients/:id` | **(admin)** Get OIDC client by ID |
| GET | `/api/oidc/clients/:id/meta` | Get OIDC client metadata (public) |
| PUT | `/api/oidc/clients/:id` | **(admin)** Update an OIDC client |
| DELETE | `/api/oidc/clients/:id` | **(admin)** Delete an OIDC client |
| PUT | `/api/oidc/clients/:id/allowed-user-groups` | **(admin)** Set allowed user groups for a client |
| POST | `/api/oidc/clients/:id/secret` | **(admin)** Rotate client secret |
| GET | `/api/oidc/clients/:id/logo` | Get client logo |
| POST | `/api/oidc/clients/:id/logo` | **(admin)** Upload client logo |
| DELETE | `/api/oidc/clients/:id/logo` | **(admin)** Delete client logo |
| GET | `/api/oidc/clients/:id/preview/:userId` | **(admin)** Preview OIDC token claims for a user |
| GET | `/api/oidc/clients/:id/scim-service-provider` | **(admin)** Get SCIM service provider for a client |
| GET | `/api/oidc/users/:id/authorized-clients` | **(admin)** List authorized OIDC clients for a user |

### Custom Claims

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/custom-claims/suggestions` | **(admin)** Get custom claim name suggestions |
| PUT | `/api/custom-claims/user/:userId` | **(admin)** Set custom claims for a user |
| PUT | `/api/custom-claims/user-group/:userGroupId` | **(admin)** Set custom claims for a user group |

### API Keys

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/api-keys` | **(auth)** List own API keys |
| POST | `/api/api-keys` | **(auth)** Create an API key |
| POST | `/api/api-keys/:id/renew` | **(auth)** Renew an API key |
| DELETE | `/api/api-keys/:id` | **(auth)** Revoke an API key |

### Audit Logs

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/audit-logs` | **(auth)** List audit logs for current user |
| GET | `/api/audit-logs/all` | **(admin)** List all audit logs |
| GET | `/api/audit-logs/filters/client-names` | **(admin)** List client names (for filtering) |
| GET | `/api/audit-logs/filters/users` | **(admin)** List usernames with IDs (for filtering) |

### Application Configuration

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/application-configuration` | List public application configuration |
| GET | `/api/application-configuration/all` | **(admin)** List all application configuration |
| PUT | `/api/application-configuration` | **(admin)** Update application configuration |
| POST | `/api/application-configuration/test-email` | **(admin)** Send a test email |
| POST | `/api/application-configuration/sync-ldap` | **(admin)** Trigger LDAP synchronization |

### Application Images

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/application-images/logo` | Get application logo (`?light=true/false`) |
| GET | `/api/application-images/email` | Get email logo |
| GET | `/api/application-images/background` | Get background image |
| GET | `/api/application-images/favicon` | Get favicon |
| GET | `/api/application-images/default-profile-picture` | **(auth)** Get default profile picture |
| PUT | `/api/application-images/logo` | **(admin)** Update application logo |
| PUT | `/api/application-images/email` | **(admin)** Update email logo |
| PUT | `/api/application-images/background` | **(admin)** Update background image |
| PUT | `/api/application-images/favicon` | **(admin)** Update favicon |
| PUT | `/api/application-images/default-profile-picture` | **(admin)** Update default profile picture |
| DELETE | `/api/application-images/background` | **(admin)** Delete background image |
| DELETE | `/api/application-images/default-profile-picture` | **(admin)** Delete default profile picture |

### SCIM

| Method | Path | Description |
|--------|------|-------------|
| POST | `/api/scim/service-provider` | **(admin)** Create a SCIM service provider |
| POST | `/api/scim/service-provider/:id/sync` | **(admin)** Trigger SCIM synchronization |
| PUT | `/api/scim/service-provider/:id` | **(admin)** Update a SCIM service provider |
| DELETE | `/api/scim/service-provider/:id` | **(admin)** Delete a SCIM service provider |

### Version

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/version/latest` | Get latest available Pocket ID version |
| GET | `/api/version/current` | **(auth)** Get currently deployed version |
