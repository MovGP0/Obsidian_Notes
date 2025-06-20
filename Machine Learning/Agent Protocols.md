| Aspect                  | [[Model Context Protocol (MCP)]]         | [[Agent Communication Protocol (ACP)]]           | [[Agent2Agent Protocol (A2A)]]                           | [[Agent Network Protocol (ANP)]]                         |
| ----------------------- | ---------------------------------------- | ------------------------------------------------ | -------------------------------------------------------- | -------------------------------------------------------- |
| **Primary Focus**       | Tool & data integration                  | Rich, multimodal messaging & governance          | Peer-to-peer task orchestration                          | Decentralized agent discovery & collaboration            |
| **Interaction Pattern** | Request–response (JSON-RPC over HTTP)    | RESTful multi-part messages, async streams       | HTTP + SSE/Webhooks, capability negotiation              | DID-based publish/subscribe with JSON-LD graphs          |
| **Discovery Mechanism** | Preconfigured endpoints                  | Service registry (optional)                      | Agent Cards exchanged at runtime                         | Decentralized identity & semantic lookup                 |
| **Security Model**      | API keys, JSON-RPC auth                  | OAuth2/OpenID Connect, audit logs                | Capability tokens, TLS                                   | Decentralized identifiers (DIDs), verifiable credentials |
| **Latency & Overhead**  | Very low (minimal framing)               | Moderate (multipart parsing, streaming setup)    | Low–medium (SSE setup, handshake)                        | Higher (graph lookups, DID resolution)                   |
| **Ideal Use Cases**     | Embedding tool access into LLM workflows | Enterprise AI chatbots, multimodal agent systems | Cross-framework orchestration, vendor-agnostic workflows | Open agent exchanges, cross-organization marketplaces    |
## Transport Options

- **Local tooling or CLI** → **stdio** (MCP, ACP via adapter)
- **Synchronous requests** → **HTTP/JSON-RPC** (all protocols)
- **Long-running workflows** → **SSE** or **HTTP streaming** (MCP, ACP, A2A)
- **Bidirectional, low-latency** → **WebSockets** (ACP, ANP, A2A)
- **Enterprise at scale** → **gRPC streams**, **message buses** (ACP, A2A)
- **Decentralized P2P** → **WebRTC**, **libp2p**, **DIDComm** (ANP, A2A)
