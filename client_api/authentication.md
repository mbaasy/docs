# Client API - Authentication

Authentication is handled by creating a Client API Access Token, accessible from the *[Mbaasy App Publisher Console](https://console.mbaasy.com') > [App] > Settings > General Settings* page.

All requests made to the Client API must include an `Authorization` header using the `Bearer` protocol`. e.g.

```sh
curl -X POST https://api.mbaasy.com/client/itunes_connect/receipts \
 -H 'Accept: application/json' \
 -H 'Content-Type: application/json' \
 -H 'Authorization: Bearer [CLIENT API ACCESS TOKEN]' \
 -d "[JSON REQUEST BODY]"
```

If you receive a `401 Unauthorized` response, check you have the correct access token and the `Authorization` header is setup correctly.
