---
icon: package
---
# API

2FAuth is built on top of its own REST API (following OpenAPI 3.1 specification), which can be used to make any other app communicate with 2FAuth.

The API provides endpoints to manage most of the 2FAuth resources:

Resource   | Description { class="compact" }
---    | ---
twofaccounts | The 2FA accounts stored in 2FAuth which you need to generate One-Time Passwords (OTP)
one-time password | The One-Time Passwords (TOTP or HOTP) generated on demand
groups | The groups used to organize 2FA accounts in 2FAuth
qrcode | Two-dimensional barcode used to encode/share 2FA accounts
icons | Images used to illustrate 2FA accounts in 2FAuth
settings | The 2FAuth user settings, which can be extended with custom settings

## Authentication

You authenticate in the 2FAuth API with a _Personal Access Token_ (PAT) built upon the OAUTH `Bearer` authentication scheme (see <a href="https://datatracker.ietf.org/doc/html/rfc6750" target="_blank">RFC 6750</a>).  
That means the PAT has to be passed via the HTTP `Authorization` header in every request made to the API.

```http
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJhdWQiOiIxIiwianRpIjoiMzZjOTc5NmFlZGI2OGQyYmE2YTIyMTE0NTN
```

As 2FAuth is designed to be used by a single user, the PAT grants access to all resources without restriction; there is no scope defined. A PAT is valid until you decide to revoke it.

### Creating an access token

Open the 2FAuth _Settings > OAUTH_ section and click the [!badge size="l" icon="plus-circle" text="Generate a new token"] link to generate a new token.

![A PAT (in green) right after its creation](/static/personal_access_token.png)

!!!warning
The token will only be shown __once__, right after its creation, so copy it __immediately__ because you won't be able to display it again.
!!!

### Revoking a token

You can revoke a personal access token by simply clicking its [!badge variant="dark" text="Revoke"] button in the _Settings > OAUTH_ section. A request made with a revoked token will receive a `401 Unauthorized` response.

!!!danger
The revocation of a token is permanent and cannot be undone.
!!!

## API documentation

The API has its own dedicated documentation that you can browse in a lightweight format below.  
You may also use the fullscreen format which provides a more comfortable layout and modern features like advanced search, mocking and more:

[!ref target="blank" icon="code-square" text="Fullscreen documentation"](/resources/rapidoc.html)

[!embed](/resources/rapidoc-embeded.html)
