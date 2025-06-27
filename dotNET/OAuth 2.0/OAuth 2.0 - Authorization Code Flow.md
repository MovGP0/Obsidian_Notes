## Authorization Code Flow
1.  The client redirects the user to the authorization server to obtain authorization.
2.  The user logs in and grants authorization to the client.
3.  The authorization server redirects the user back to the client with an authorization code.
4.  The client exchanges the authorization code for an access token.

```mermaid
sequenceDiagram
    participant Client
    participant User
    participant Authorization Server
    participant Resource Server

    Client->>Authorization Server: Redirect to authorization endpoint
    loop Until authorization granted
        User->>Authorization Server: Login and grant authorization
    end
    Authorization Server-->>Client: Redirect back with authorization code
    Client->>Authorization Server: Exchange authorization code for access token
    Client->>Resource Server: Send access token
    Resource Server-->>Client: Return protected resource
```
