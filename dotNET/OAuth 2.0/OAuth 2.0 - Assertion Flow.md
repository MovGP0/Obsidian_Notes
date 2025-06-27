## Assertion Flow

1.  The client sends an assertion to the authorization server to request an access token.
2.  The authorization server authenticates the client and verifies the assertion, and returns an access token to the client.

```mermaid
sequenceDiagram
    participant Client
    participant Authorization Server
    participant Resource Server

    Client->>Authorization Server: Request access token with assertion
    Authorization Server-->>Client: Return access token
    Client->>Resource Server: Send access token
    Resource Server-->>Client: Return protected resource
```