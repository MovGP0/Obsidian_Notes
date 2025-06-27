| Role           | Technology     |
| -------------- | -------------- |
| Authentication | OpenID Connect |
| Authorization  | OAuth          |

## Flows

| Flow                                                     | Description                                                                                                                                                                                                       |
| -------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [[OAuth 2.0 - Authorization Code Flow]]                  | used by web applications that need to access an API on behalf of a user.                                                                                                                                          |
| [[OAuth 2.0 - Implicit Flow]]                            | similar to the authorization code flow, but it does not involve an authorization code exchange. It is used by single-page applications (SPAs) and native applications that cannot securely store a client secret. |
| [[OAuth 2.0 - Resource Owner Password Credentials Flow]] | used by trusted clients, such as those owned by the resource owner, to directly request an access token using the resource owner's credentials.                                                                   |
| [[OAuth 2.0 - Client Credentials Flow]]                  | used by clients to request an access token for their own resources, without the involvement of a user.                                                                                                            |
| [[OAuth 2.0 - Device Code Flow]]                         | used by device-based client applications that are unable to receive user interaction, such as smart TVs or IoT devices.                                                                                           |
| [[OAuth 2.0 - Assertion Flow]]                           | used by clients that have their own trust relationship with the authorization server, such as a SAML 2.0 identity provider.                                                                                       |
| [[OAuth 2.0 - Refresh Token Flow]]                       | used by clients to obtain a new access token after the original access token has expired.                                                                                                                         |

## Roles

| Role                 | Description                                                                                  |
| -------------------- | -------------------------------------------------------------------------------------------- |
| Resource Owner       | has access to a protected resource; can grant access to that resource to others (ie. a user) |
| Resource Server      | hosts the protected resource                                                                 |
| Client               | application that wants to access the protected resource on behalf of the resource owner      |
| Authorization Server | server that is responsible for issuing access tokens to clients                              |

## Authorization Server Endpoints

| Endpoint               | Description                                                                                                                                                                                                                                          |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Authorization endpoint | the endpoint that the client uses to obtain authorization from the resource owner. It is usually implemented as a web page that prompts the resource owner to grant access to the protected resource                                                 |
| Token endpoint         | the endpoint that the client uses to request an access token from the authorization server. It is typically implemented as an HTTP POST request that exchanges an authorization code or refresh token for an access token.                           |
| Introspection endpoint | the endpoint that the client can use to determine the status of an access token. It is typically implemented as an HTTP POST request that returns information about the access token, such as its expiration time and the scope of access it grants. |
| Revocation endpoint    | This is the endpoint that the client can use to revoke an access token or refresh token. It is typically implemented as an HTTP POST request that invalidates the token.                                                                             |

## Links
- [OAuth 2.0 Specification](https://oauth.net/2/)
- [Forget about OAuth 2.0. Here comes OAuth 2.1](https://www.youtube.com/watch?v=Z9DJzVJD_vg) (YouTube)

## Libraries
- [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) OAuth 2.0 social authentication providers for ASP.NET Core

### Related Libraries
- [NWebSec](https://docs.nwebsec.com/en/latest/) - Security libraries for ASP.NET Core