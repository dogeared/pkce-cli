## PKCE Command Line 

This tool demonstrates the Authorization Code Flow with PKCE 

It follows these steps:

1. Creates a random string called the `code verifier`
2. Hashes the `code verifier` creating a value called the `code challenge`
3. Builds an authorization URL which includes:
    a. Okta OIDC Client ID
    b. a list of request scopes
    c. a redirect uri
    d. a randomly generated state value
    e. the `code challenge`
    f. a response type set to `code` to indicate that we're using the authorization code flow
4. Launches a browser with the authorization URL, at which point you will authenticate
5. Receives the redirect from the authorization url, which includes a `code`
6. Calls the `token` endpoint with:
    a. grant type set to `authorization code`
    b. a redirect uri that must match the one used in the authorization step
    c. Okta OIDC Client ID
    d. the `code`
    e. the `code verifier` from earlier
7. Displays the tokens returned from the `token` endpoint
8. Uses the returned access token to call the `userinfo` endpoint

## Usage

```
Usage: pkce-cli [options]

Options:
  -c, --client_id <okta client id>               OIDC Client ID (default: "")
  -o, --okta_org <okta org url>                  ex: https://micah.oktapreview.com (default: "")
  -s, --scopes <space separated list of scopes>  Space separated list of scopes (default: "")
  -r, --redirect_uri <redirect uri>              redirect uri (default: "")
  -h, --help                                     output usage information
```

## Run

```
./pkce-cli \
  --client_id 0oahdifc72URh7rUV0h7 \
  --okta_org https://micah.oktapreview.com \
  --scopes "openid profile email" \
  --redirect_uri http://localhost:8080/redirect 
```
