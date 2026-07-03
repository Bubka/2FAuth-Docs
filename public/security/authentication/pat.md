# Personal Access Token

Use Personal Access Tokens (PAT) to authenticate the [2FAuth Web Extension](/#browser-extensions) or to authenticate requests sent to the 2FAuth REST API.

## Creating an access token

Open the 2FAuth _Settings > OAUTH_ section and click the [!badge size="l" icon="plus-circle" text="Generate a new token"] link to generate a new token.

![A PAT (in green) right after its creation](/static/personal_access_token.png)

!!!warning
The token will only be shown __once__, right after its creation, so copy it __immediately__ because you won't be able to display it again.
!!!

## Revoking an access token

You can revoke a personal access token by simply clicking its [!badge variant="dark" text="Revoke"] button in the _Settings > OAUTH_ section. A request made with a revoked token will receive a `401 Unauthorized` response.

!!!danger
The revocation of a token is permanent and cannot be undone.
!!!

[!ref How to authenticate API requests](/api/#authentication)
