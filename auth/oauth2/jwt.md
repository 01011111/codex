---
title: JSON Web Token
---

# JSON Web Token

[RFC7519](https://tools.ietf.org/html/rfc7519)

## JOSE (JSON Object Signing and Encryption) Header

It contains metadata about the type of token and the cryptographic algorithms used to secure its contents.

Parameter Key | Value
---: | :---
`alg` | Short name of the algorithm used for signature
`kid` | ID of the key used for signing in case of the use of an asymmetric algorithm
`typ` | Shortname describing the type of token

## JWS (JSON Web Signature) Payload

It contains verifiable security statements, such as the identity of the user and the permissions they are allowed.

Claim Name | Value | Required By
---: | :--- | :---
`aud` | audience - audience that this JWT is intended for, usually the client id | OIDC
`exp` | expiration - expiration time on or after which the ID Token MUST NOT be accepted for processing | OIDC
`iat` | issued at - time at which the JWT was issued  | OIDC
`iss` | issuer - full https uri representing the identity provider that created the JWT | OIDC
`nonce` | a case-sensitive string provided by the client at authorization time | OIDC
`scope` | comma separated list of elements the client requested access to |
`sub` | subject - Unique Identifier of the entity the JWT was produced for | OIDC

## JWS (JSON Web Signature) Signature

It is used to validate that the token is trustworthy and has not been tampered with. When you use a JWT, you **must** check its signature before storing and using it.

The signature is generated based on this plain text: `base64UrlEncode(header) + "." + base64UrlEncode(payload)`
