<div align="center">
  <a href="https://www.buildwithfern.com/?utm_source=github&utm_medium=readme&utm_campaign=docs-starter-openapi&utm_content=logo">
    <img src="/fern/docs/assets/fern-logo.png" height="50" alt="Fern Logo" />
  </a>
</div>

# Docs RBAC Demo

This website builds off of the [Docs Starter](https://github.com/fern-api/docs-starter) template and is a demo for the [RBAC](https://buildwithfern.com/learn/docs/building-and-customizing-your-docs/rbac) feature.

## Key Files

- `fern/docs.yml` - The configuration file for the docs that contains RBAC roles and viewer permissions

## Live Demo

View the site at [plantstore-rbac.ferndocs.app](https://plantstore-rbac.ferndocs.app)

## How to Configure RBAC for Your Own Docs

1. Define your roles in [`fern/docs.yml`](https://github.com/fern-demo/rbac-example/blob/22d72c53d4f397bbef70b3e1c69e2e27995a363f/fern/docs.yml#L5-L9)
2. Add to the navigation in [`fern/docs.yml`](https://github.com/fern-demo/rbac-example/blob/22d72c53d4f397bbef70b3e1c69e2e27995a363f/fern/docs.yml#L28-L31)
3. Set up auth (requires contacting [Fern support](https://buildwithfern.com/learn#get-support))
   a. We will send you a unique JWT Secret which you will use to sign the token (using HS256)
   b. You will send us the `issuer` and Fern will use it to also verify the token
4. Generate the JWT token and set it using `/api/fern-docs/auth/jwt/callback?fern_token=<token>` (this will set the `fern_token` cookie, however you in production you may want to set this cookie without using this callback.)

## How RBAC works

- `viewers: everyone` - This role is special and includes all users, whether they are logged in or not. You do not need to include this role in the JWT token.
- `viewers: []` - This is default, and means pages are visible only to authed users (regardless of role).
- `viewers: [role1, role2]` - Pages are visible to users with `role1` or `role2` (which is interpreted [disjunctively](https://en.wikipedia.org/wiki/Logical_disjunction)).
- `orphaned: true` - Stops role inheritance from the parent sections (which is otherwise inherited [conjunctively](https://en.wikipedia.org/wiki/Logical_conjunction)).

## Testing the JWT Token

1. Go to [jwt.io](https://jwt.io/)
2. Click on `JWT Encoder` tab (not `JWT Decoder`)

Generate a HS256 JWT token using the following payload:

### Header

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

### Payload

```json
{
  "fern": {
    "roles": []
  },
  "iss": "https://plantstore-rbac.ferndocs.app"
}
```

Note: you can set the expiration date of the token by adding the `exp` claim in the payload.

### Secret

In the demo application for `plantstore-rbac.ferndocs.app` the secret is: `BtoR10mgJjaQIsToN7rph7W+NCTdA0wUPYMQNwIGo7k=`. Keep the encoding format as `Base64`.

### Testing multiple roles with `plantstore-rbac.ferndocs.app`:

(These secrets are used for this demo only, and will not work for your application. Please keep your JWT secrets securely stored, and always include the `exp` claim—which is missing in this example—in the JWT payload for your application.)

- `roles: []`: https://plantstore-rbac.ferndocs.app/api/fern-docs/auth/jwt/callback?fern_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmZXJuIjp7InJvbGVzIjpbXX0sImlzcyI6Imh0dHBzOi8vcGxhbnRzdG9yZS1yYmFjLmZlcm5kb2NzLmFwcCJ9.gL714nwkGNonAM3R9MsvODqjJcbeNh3oz3SNYY16ZG0 (note: this is the default authed role)
- `roles: ["admin"]`: https://plantstore-rbac.ferndocs.app/api/fern-docs/auth/jwt/callback?fern_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmZXJuIjp7InJvbGVzIjpbImFkbWluIl19LCJpc3MiOiJodHRwczovL3BsYW50c3RvcmUtcmJhYy5mZXJuZG9jcy5hcHAifQ.glMqba0rB9ExWG4xogy7GuOAMyTuAEo-KbIHxCbdRn4
- `roles: ["partner"]`: https://plantstore-rbac.ferndocs.app/api/fern-docs/auth/jwt/callback?fern_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmZXJuIjp7InJvbGVzIjpbInBhcnRuZXIiXX0sImlzcyI6Imh0dHBzOi8vcGxhbnRzdG9yZS1yYmFjLmZlcm5kb2NzLmFwcCJ9.fJUnQfr5LCujLil1hkV7FgaE0HV7aI6F3qfO2Hd5iO4
- `roles: ["beta-user"]`: https://plantstore-rbac.ferndocs.app/api/fern-docs/auth/jwt/callback?fern_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmZXJuIjp7InJvbGVzIjpbImJldGEtdXNlciJdfSwiaXNzIjoiaHR0cHM6Ly9wbGFudHN0b3JlLXJiYWMuZmVybmRvY3MuYXBwIn0.8OF7khA17e0smHn_afkmtdvrTJLUuHeN_gC_HF5cP40
- `roles: ["admin", "beta-user"]`: https://plantstore-rbac.ferndocs.app/api/fern-docs/auth/jwt/callback?fern_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmZXJuIjp7InJvbGVzIjpbImFkbWluIiwiYmV0YS11c2VyIl19LCJpc3MiOiJodHRwczovL3BsYW50c3RvcmUtcmJhYy5mZXJuZG9jcy5hcHAifQ.75UeF6ogS3gbXaN0ySKIa13aanYok8N1yqFgAVQNRxE
- `roles: ["admin", "partner", "beta-user"]`: https://plantstore-rbac.ferndocs.app/api/fern-docs/auth/jwt/callback?fern_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmZXJuIjp7InJvbGVzIjpbImFkbWluIiwicGFydG5lciIsImJldGEtdXNlciJdfSwiaXNzIjoiaHR0cHM6Ly9wbGFudHN0b3JlLXJiYWMuZmVybmRvY3MuYXBwIn0.5OhbcCMKlJGiiCKn5VYX2-Y7Iy_jIIvO9zkiGgQ8GOA

Note: you do not need to include the special `everyone` role in the JWT token because it is implicitly included. Non-authed users will also be able to view pages that are targeted by the `everyone` role. And content that is not tagged with a role will be viewable only by authed users.

Note: copy the JWT tokens above into `jwt.io` in the `JWT Decoder` tab to explore the payloads.

## Glossary

- `roles`: list of roles (we recommend that these nouns should be singular, e.g. `user` not `users`, to make it easier to reason about: `user is a viewer of this page`)
- `viewers`: list of roles applied on sections, pages, etc.
- `everyone`: special role that includes all users, whether they are logged in or not.
- `orphaned`: a navigation item in the navigation tree will not inherit roles from its parent sections.
- `disjunctive`: OR logic
- `conjunctive`: AND logic
