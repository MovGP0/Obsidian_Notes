
## Resource Owner Password Credentials Flow

1.  The client sends the resource owner's username and password to the authorization server.
2.  The authorization server authenticates the resource owner and returns an access token to the client.

```mermaid
sequenceDiagram
    participant Client
    participant Authorization Server
    participant Resource Server

    Client->>Authorization Server: Request access token with client ID and secret
    Authorization Server-->>Client: Return access token
    Client->>Resource Server: Send access token
    Resource Server-->>Client: Return protected resource
```