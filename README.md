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
npm install
./pkce-cli \
  --client_id 0oahdifc72URh7rUV0h7 \
  --okta_org https://micah.oktapreview.com \
  --scopes "openid profile email" \
  --redirect_uri http://localhost:8080/redirect 
```

You'll get output like this:

```
Created Code Verifier: 0131_98be_369c_b875_2a21_7b0b_4bd3_72dc_959c_2ec8_1362
Created Code Challenge: WNaPtVTER9Dp05EUkRgiKL-BiFXsI5755ieyhJzpGgA
Calling Authorize URL: https://micah.oktapreview.com/oauth2/v1/authorize?client_id=0oahdifc72URh7rUV0h7&response_type=code&scope=openid+profile+email&redirect_uri=http://localhost:8080/redirect&state=e54c_1a08_5ae2_f0aa_67c4_2459_c078_f812_ea38_cf58_33fe&code_challenge_method=S256&code_challenge=WNaPtVTER9Dp05EUkRgiKL-BiFXsI5755ieyhJzpGgA
Got code: NloXE9rRhnNmA_RxnJ53
Calling /token endpoint with form:
{ grant_type: 'authorization_code',
  redirect_uri: 'http://localhost:8080/redirect',
  client_id: '0oahdifc72URh7rUV0h7',
  code: 'NloXE9rRhnNmA_RxnJ53',
  code_verifier: '0131_98be_369c_b875_2a21_7b0b_4bd3_72dc_959c_2ec8_1362' }
Got token response:
{ access_token:
   'eyJraWQiOiItVV92MHBJVGx5X0V3MTJfTzZuT1lWb081ZVBucm1Iek9wbkxfS2FHN0lzIiwiYWxnIjoiUlMyNTYifQ.eyJ2ZXIiOjEsImp0aSI6IkFULjJhaUtHVjVhR3N4VTlvZDZHWTZTTkh5dF9NMzU5cW9hYVFzdzRqQllRdUkiLCJpc3MiOiJodHRwczovL21pY2FoLm9rdGFwcmV2aWV3LmNvbSIsImF1ZCI6Imh0dHBzOi8vbWljYWgub2t0YXByZXZpZXcuY29tIiwic3ViIjoibWljYWhAYWZpdG5lcmQuY29tIiwiaWF0IjoxNTQxOTE5Mzk2LCJleHAiOjE1NDE5MjI5OTYsImNpZCI6IjBvYWhkaWZjNzJVUmg3clVWMGg3IiwidWlkIjoiMDB1ZG8zYmFseE5od0tkU0wwaDciLCJzY3AiOlsiZW1haWwiLCJvcGVuaWQiLCJwcm9maWxlIl19.jjBWPnlNac0FVhDwepsIqhKFKdFHsipmq9VZMI0Xti3VWLtldcalogW425Di1oqMzYpeC6c6x_wGsbS260QtwHGEMm96TE_DTU8YUigRd7zHM-yuMEb_yrF4GJI7LEOViFx0PABdnvj5zfmzR1U2qDGC_BMorjRYd1SrOKZldxtd6Rq1_PSwwvsr77CNBdDfrLowzw5lCKbAIZLon9bR7VxMZPPiGDR3ky9SoKZQyjs6tkbC1ySvW3Rd5kp6-LdZkcEelP7Q4SP0io4dwJVuTX6Buo5jJw479L7oWwjuC827vrQheBE5JsKg7Ijh_gwbUdHlJBgA8wcc7cNUpBYWig',
  token_type: 'Bearer',
  expires_in: 3600,
  scope: 'email openid profile',
  id_token:
   'eyJraWQiOiJUcGwtaVowcHhtQWpZb05ISlVSLUtjWkdCMGdUTWFUYkd1clQwd19GMXgwIiwiYWxnIjoiUlMyNTYifQ.eyJzdWIiOiIwMHVkbzNiYWx4Tmh3S2RTTDBoNyIsIm5hbWUiOiJNaWNhaCBTaWx2ZXJtYW4iLCJlbWFpbCI6Im1pY2FoQGFmaXRuZXJkLmNvbSIsInZlciI6MSwiaXNzIjoiaHR0cHM6Ly9taWNhaC5va3RhcHJldmlldy5jb20iLCJhdWQiOiIwb2FoZGlmYzcyVVJoN3JVVjBoNyIsImlhdCI6MTU0MTkxOTM5NiwiZXhwIjoxNTQxOTIyOTk2LCJqdGkiOiJJRC5LNF9Xb2ktUlc0WXpDaDZBaW02NHViME5DLUxObG5JZTBmUE82cVhQSnE0IiwiYW1yIjpbInB3ZCJdLCJpZHAiOiIwMG85dmFza2sySnQyWEZWZzBoNyIsInByZWZlcnJlZF91c2VybmFtZSI6Im1pY2FoQGFmaXRuZXJkLmNvbSIsImF1dGhfdGltZSI6MTU0MTkxNjczNywiYXRfaGFzaCI6InU4YjBTZTlsOW9oZU5oVFd4akdEWkEifQ.T4dTeiYljVBqXL-hBPAyP9UFmSRuA7yuanLFgSGTvy7zvU_xEAaT4fBmUY5jPsV3vGsLOMe5fOU3PYyK1kbzR717RJRDnLmFAkqDbYixjaoVGpvyM6cr08QFAjT0bgYkiX5h1eagRyIZeSrpvYZSV0T4ChDnMzQYDC_GVl9C3TF-8dgXai8Xn-e4r78z9CTi04JDENJrFhyt7Rj7iqYMXJUGycnihnIJwnbUafUYVIWW370TlaQu5nmgMV7g5zgEXjIUBq3lplioGQutAxr_tTOkrSeQD7jea22aZRu41moOJa-HxfvsWqOYHMq1OC9nsTWy9ra5H5vQE1rtXFhV0Q' }
Calling /userinfo endpoint with access token
{ sub: '00udo3balxNhwKdSL0h7',
  name: 'Micah Silverman',
  profile:
   'https://www.facebook.com/app_scoped_user_id/10156159259014459/',
  locale: 'en-US',
  email: 'micah@afitnerd.com',
  preferred_username: 'micah@afitnerd.com',
  given_name: 'Micah',
  family_name: 'Silverman',
  zoneinfo: 'America/Los_Angeles',
  updated_at: 1541796005,
  email_verified: true }
```
