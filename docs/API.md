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
preferences | The user preferences
settings | The 2FAuth administrator settings, which can be extended with custom settings

---

## Authentication

You authenticate in the 2FAuth API with a _Personal Access Token_ (PAT) built upon the OAUTH `Bearer` authentication scheme (see <a href="https://datatracker.ietf.org/doc/html/rfc6750" target="_blank">RFC 6750</a>).  
That means the PAT has to be passed via the HTTP `Authorization` header in every request made to the API.

```http
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJhdWQiOiIxIiwianRpIjoiMzZjOTc5NmFlZGI2OGQyYmE2YTIyMTE0NTN
```

A PAT is valid until you decide to revoke it.

### Creating an access token

{{ include "create-pat" }}

### Revoking a token

{{ include "revoke-pat" }}

---

## API documentation

The API has its own dedicated documentation that you can browse in a lightweight format below.  
You may also use the fullscreen format which provides previous versions, a more comfortable layout and modern features like advanced search, mocking and more:

[!ref target="blank" icon="code-square" text="Fullscreen documentation"](/resources/rapidoc.html)

[!embed](/resources/rapidoc-embeded.html)
