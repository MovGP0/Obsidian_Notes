
## Client Credentials Flow

1.  The client sends its own credentials (client ID and secret) to the authorization server.
2.  The authorization server authenticates the client and returns an access token to the client.

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