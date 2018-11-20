# GraphQL API - Authentication

Programmatic access to the GraphQL API requires authentication via a Service Account furnished [JSON Web Token](https://en.wikipedia.org/wiki/JSON_Web_Token).

To use the GraphQL API with a service account, you must generate a JWT, signed with the private key you receive when you create a service account.

It is important to note that **Mbaasy does not save the private key** anywhere on our system, we make it available to you only on creation. If you loose your private key you will need to create a new service account.

A JWT Token consists of three Base64 encoded parts separated by a period:

1. **Header:** Containing the algorithm (`alg`) used to sign the JWT. In our case we use `RS256`.
1. **Payload:** Containing the audience ([`aud`](https://tools.ietf.org/html/rfc7519#section-4.1.3)), subject ([`sub`](https://tools.ietf.org/html/rfc7519#section-4.1.2)), expiration ([`exp`](https://tools.ietf.org/html/rfc7519#section-4.1.4)), etc.
1. **Signature:** A cryptograpic signature made up from the previous two parts and signed with the private key.

This is an example of a JWT Token:

`eyJhbGciOiJSUzI1NiJ9.eyJhdWQiOiJTZXJ2aWNlQWNjb3VudCIsInN1YiI6ImRhZDM2OTdmLWY1MjQtNDZiYi1hMTA3LTg4NjQwZDMxYzRkMCJ9.OyFO4fOGVRu4Bc62YvHgcbtIsElOODgTgSCRHs1ZhDS-0_xpmADOmHA93w1aqH2c0bnMnth5jiYkOKr3GI_O-uCwYFlbgHMvDs4T6mRXhVMy4Rzt0hLzG23gCPhkfuQwzu23R4t7WdnrjVToMrMFD24OO1w9FTZbeOwig9-KhRX3QFczgTjD8ywP1OMuD1336xzlgiZjUZEvOxhSsFnsCUZl3DUG5xoRXzxOAZpbkPB38K4Sstnv742rlgWMcDocUljxkf1mWagNPkP9-D9-I07auEhoz5_wDiBsq-7KQNVusHuxc8qUMQXMjnX5nk57omDCjZc23E_gtsnHqOp5ww`

Base64 decoding the first (Header) part we get:

```json
{
  "alg": "RS256"
}
```

We can see in the Header the `alg` is set to `RS256`.

Base64 decoding the second (Payload) part we get:

```json
{
  "aud": "ServiceAccount",
  "sub": "dad3697f-f524-46bb-a107-88640d31c4d0"
}
```

We can see in the Payload the [`aud`](https://tools.ietf.org/html/rfc7519#section-4.1.3) is set to `ServiceAccount` and the sub is set to `dad3697f-f524-46bb-a107-88640d31c4d0`, this is the **Service Account ID**.

Optionally, payloads can accept the [`iat`](https://tools.ietf.org/html/rfc7519#section-4.1.6), [`nbf`](https://tools.ietf.org/html/rfc7519#section-4.1.5)  and [`exp`](https://tools.ietf.org/html/rfc7519#section-4.1.4) properties, and will be validated with a 1 minute leeway to allow for clock skew. We **strongly recommend** including these properties to guard against [sniffing attacks](https://en.wikipedia.org/wiki/Sniffing_attack) and [replay attacks](https://en.wikipedia.org/wiki/Replay_attack). To avoid the complexity of caching the JWT while significantly improving security, you could generate a new JWT for each request with an exp of around 5 to 10 seconds.

The third part (Signature) contains no human readable information so we won't decode it. It is created by combining the first two parts and creating a cryptograpic signature using the private key.

In this example we are using the following private key:

`MIIEogIBAAKCAQEAld2s5NRdQHa5XIIGAUXb8tZd/4N8eeT0MPOvbvDz0mfzAGvc8QN4RSJsDbwdDISTp3lAow2ARCwfEe8mHxCUDoJNguxXy8QC3m9tBR+FOmxa+IHrPls5yVgcJf664OmMYUiV28ANq/kfq/bB3dCpuEDqeEhcyvykQDAwAtVwFGI/OXaxvmNVI/2Vc3sBLxGPNlMXZjP9iqJ8XPoA0cwWqAXpigpqgU51NVEkkYB6aLz270YEg+2YMArC1Tq8pKI7t3MolBy409DcTD9FW2qQ5AUqLozuIxBxh+6RVL2shRC+k5z01cNEmY++rgUwjRa7EjduysIlrmzlRzjzH+AS6QIDAQABAoIBAHsars0BjOBJJhZRUDF3eydM48Xh8jwG0HftlGwjTYXGkZwE/maUHRVdIzldDtebh9aICYpmqrhVLeiDc+MMsTIB4z3qemwNRPYGvZz5uWy2WTvNLLn6Icu8rtKfHk9mSUQYZdEvP4vGhjex6KoWn3OVD4Vim6a6RQndC3ModHqov8FMNVnJ5pPGbGLCgIML1E/uMBkILDGZtJKf1IebTSLB0N6rMUR9gvXfI2zHuzhdMrTnJsSC8/i1kEDPmB+mVel6Jx76DLox9hre7hP+9eZqk6tQDsTVEaIcryhkJF9Nzl/8eHdFx587HcQx3CiMDQ6AoOfQNCsPjPmAGs1NXwECgYEAx4ZYJuvoDVMz1j8aXCWWRvbCk7R9uwqU9H6Ayod/5aDsS8VAa55W1OguPcwwBJ1LnIoRGyZSuLrDC0Mno50nQLoqe/aOsPmX7EQ8uAu96Akx9i1KMA/kqYIQ4J/kZh26Hv8R3wa0tbbMudxigvImL+4f6Yw2I4bupEjAQATF1NkCgYEAwEkHBOlKtSIg5JELtTn3MN9BsIC9bz1dOs/HCtQ0LtSaZxbbUxFFcgtLDK5NaD3REsnlt6eb9G5NLmBwWv1Za0/WLAGSznQ+xplAszUAX5jAdQVN11JVHNpX6n0LDOxrxtdrLLFc/SEU/xjCJb4g10CsTxkDKKXL/owSSWqEJJECgYByi3XgIl6+B54iyWdgr1NuugtKsLvdvV12X4mgu9l5AsGlXkB1iBlhcUnawHZwr7zQyZK4e2/SDaYbuRnwnDAEwNvS/uE8SI4nXaVeI5+Krny4T5ffr8AecEUwL5r35OkNEnI4D6l/CMrPiO4lLs8thQ9dDNmn27b1Ka71EchhQQKBgGx5xkfzBH5jHoNTgHNgnB8qE/jq5/OVfr7q8LMfO8Efl2uI7XpTSAYqcNBalLi2Bvi+haWyHL0sbMY5CamLO6Lai0yQJq3pznBjjKiMwgUsO4bI0z0h6Xo5g1d5634C8lpetMul03vJ6fpvkTeRpx1IEu0cgzevMQINK1PXj2zBAoGAEBtJOmpRe/PLdflqeHhz7K9d2G/OC7vlPj/b/VFylQtrd2Csk5tLNd/1S4LU2EqftRFXdpdp4fgNw6DIszTRgUe8sClNSHpcI0j+HGjgifSvVix+ltC/rEpgGNWAnlPRm2QT8AQc9yrlEpmWOMZn0dpYPTiRE1iCbMxRGEgP7sQ=`

Once you have created a JWT token you can use it to access the GraphQL API by adding it to the `Authorization` header with the `Bearer` protocol. e.g.

```sh
curl -X POST \
     -H "Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJhdWQiOiJTZXJ2aWNlQWNjb3VudCIsInN1YiI6ImRhZDM2OTdmLWY1MjQtNDZiYi1hMTA3LTg4NjQwZDMxYzRkMCJ9.OyFO4fOGVRu4Bc62YvHgcbtIsElOODgTgSCRHs1ZhDS-0_xpmADOmHA93w1aqH2c0bnMnth5jiYkOKr3GI_O-uCwYFlbgHMvDs4T6mRXhVMy4Rzt0hLzG23gCPhkfuQwzu23R4t7WdnrjVToMrMFD24OO1w9FTZbeOwig9-KhRX3QFczgTjD8ywP1OMuD1336xzlgiZjUZEvOxhSsFnsCUZl3DUG5xoRXzxOAZpbkPB38K4Sstnv742rlgWMcDocUljxkf1mWagNPkP9-D9-I07auEhoz5_wDiBsq-7KQNVusHuxc8qUMQXMjnX5nk57omDCjZc23E_gtsnHqOp5ww" \
     -d '{"query": "{ viewer { ...on ServiceAccount { uuid } } }"}' \
     https://api.mbaasy.com/graphql
```

## Additional resources

* [Wikipedia article](https://en.wikipedia.org/wiki/JSON_Web_Token)
* [RFC specification](https://tools.ietf.org/html/rfc7519)

### JWT Libraries

* [Ruby Library](https://github.com/jwt/ruby-jwt)
* [Node.js Library](https://github.com/auth0/node-jsonwebtoken)
* [Python Library](https://github.com/jpadilla/pyjwt)
* [Java Library](https://github.com/auth0/java-jwt)
* [Swift Library](https://github.com/auth0/JWTDecode.swift)

### Examples

* [Ruby example](https://gist.github.com/marcgreenstock/2529c9d19eef20d1b5b75619cf3a71cb)
