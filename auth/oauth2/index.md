---
title: OAuth2
---

# OAuth2

> OAuth 2.0 is the industry-standard protocol for authorization. OAuth 2.0 focuses on client developer simplicity while providing specific authorization flows for web applications, desktop applications, mobile phones, and living room devices. This specification and its extensions are being developed within the [IETF OAuth Working Group](https://www.ietf.org/mailman/listinfo/oauth).

OAuth2 leverages several core elements:

## Grant Types (Flows)

Grant Types in OAuth2 defines how the Resource Owner (user), the Third Party, and the Authorization Server interacts with each other.

There are different types that caters to different scenarios. Some of them are deprecated as they are not considered secure and should be rolled out in favor or newer flows. 

## JSON Web Tokens

It is a specific format using JSON, encoded to base64 stripped of padding used to exchange information. It is heavily relied on by OAuth2 as proof of authorization, among other uses.

## JSON Web Key

It is a specific format using JSON to represent a cryptographic public key. It provides a way to uniquely identify a key and convert it to a more common format for signature verification purposes.

A JSON Web Key Set is simply a collection of JWK, it is used so that keys can be rotated without interruption of service.

## OpenID Connect

> An open standard for authentication that allows applications to verify users are who they say they are without needing to collect, store, and therefore become liable for a user’s login information.

It is a superset of OAuth2 that adds conventions around user identification.

## Scopes

> Scope is a mechanism in OAuth 2.0 to limit an application's access to a user's account. An application can request one or more scopes, this information is then presented to the user in the consent screen, and the access token issued to the application will be limited to the scopes granted.

Scopes allow the user to have granularity on what they grant access to.

## More Details

- [Grant Types]({{ site.baseurl }}/auth/oauth2/grant/
- [JSON Web Key Set]({{ site.baseurl }}/auth/oauth2/jwks)
- [JSON Web Token]({{ site.baseurl }}/auth/oauth2/jwt)
- [OpenID Connect]({{ site.baseurl }}/auth/oauth2/oidc/
- [Scopes]({{ site.baseurl }}/auth/oauth2/scopes)
