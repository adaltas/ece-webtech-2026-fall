---
duration: 1h
---

# OAuth and OpenID Connect | with Supabase Auth

[OAuth 2.0](https://oauth.net/2/) is designed only for **authorization**, for granting access to data and features from one application to another. OpenID Connect (OIDC) is a thin layer that sits on top of OAuth 2.0 that adds login and profile information about the person who is logged in (**authentication**).

This class goes deep into the protocol, how it works, and how it integrates with traditional web applications as well as with microservice applications - and with Supabase in particular.

## OAuth basics

- A meta-framework, a protocol of protocols
- Used as the base of other specifications (OpenID Connect, UMA, HEART, ...)
- OAuth is OAuth2
- Addresses some important requirements
  - Delegate access
  - No password sharing
  - Revocation of access

## OAuth actors

- Resource Owner (RO), (end user, organization, ...)
- Client (web app, mobile app, ...)
- Authorization Server (AS)
- Resources Server (RS) (API, ...)

## OAuth process

- Request access (signup, login, ...)
- Login (transparent, username/password, 2-factor authentication, ...)
- Consent (allowing the application to access resources on the user's behalf, can be hidden)

## Authorization Code Flow

- Multiple flows in OAuth, even more in OpenID Connect
- Most popular is Authorization Code Flow
- Application requires the user to be authenticated
- User is redirected from the application to the Authorization Server
- Once authenticated, consent takes place
- User authorizes the application to consume resources on his behalf
- User is redirected to the application with a short-lived authorization code
- Authorization code is a Nonce (No More than Once) code
- Code is exchanged for an access token from the client application

## Scopes

- Determine what to access
- Grouping of claims
- Claims in scope can be used as permissions
- No standardized scope in OAuth, various in OpenID Connect

## Kind of tokens

- Access Tokens, like a session, used to secure API calls
- Refresh Tokens, like a password, used to get a new access token

## JWT (JSON Web Token)

- Not the only kind of token supported by OAuth
- JWT is the most popular
- Pronounced like the English word "jot"
- Lightweight tokens passed in HTTP headers & query strings
- Encoded as JSON
- Encrypted, signed, or neither
- 3 parts, the header, the payload, the signature

## Usages of OAuth

- Not for authentication
- Not for federation
- Not really for authorization
- For delegation

## OpenID Connect

- Based on OAuth2
- Clients also receive ID Tokens (identity tokens)
- User info endpoint to get user data
- Additional flow
- Claims, request obj, ...
- ID Token for the client shall never be sent (eg to microservices)

## PKCE

- Pronounced like the English word "pixy"
- Extension to the Authorization Code flow
- Securely perform the OAuth exchange from public clients (no client secret)
- [OAuth 2.1](https://oauth.net/2.1/) (draft) makes it mandatory for every client and removes the implicit flow
- Default flow in Supabase Auth for server-side rendering
- Wherever the implicit flow is still found: never use it

## Resources

- [An Illustrated Guide to OAuth and OpenID Connect](https://developer.okta.com/blog/2019/10/21/illustrated-guide-to-oauth-and-oidc), video and article, 16 min.
- [Securing APIs and Microservices with OAuth and OpenID Connect](https://curity.io/resources/videos/securing-apis-and-microservices-with-oauth-and-openid-connect/), video, 45 min.
- [PKCE, RFC 7636: Proof Key for Code Exchange](https://oauth.net/2/pkce/)
- [What's going on with the OAuth 2.0 Implicit flow?](https://www.youtube.com/watch?v=CHzERullHe8)

---

*The content of this document, including all text, images, and associated materials, is the exclusive property of Adaltas and is protected by applicable copyright laws. Unauthorized distribution, reproduction, or sharing of this content, in whole or in part, is strictly prohibited without the express written consent of the author(s). Any violation of this restriction may result in legal action and the imposition of penalties as prescribed by law.*
