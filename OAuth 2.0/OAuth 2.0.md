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

## Links
- [OAuth 2.0 Specification](https://oauth.net/2/)
- [Forget about OAuth 2.0. Here comes OAuth 2.1](https://www.youtube.com/watch?v=Z9DJzVJD_vg) (YouTube)

## Libraries
- [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) OAuth 2.0 social authentication providers for ASP.NET Core

### Related Libraries
- [NWebSec](https://docs.nwebsec.com/en/latest/) - Security libraries for ASP.NET Core