- binary Serialization protocol by Google
- High-Performance / Real-Time applications
- Requires HTTP/2 (TLS)
- Available in .NET 5+
- Data structure is defined in `.proto` files
- Used for Microservice-to-Microservice communication; use a REST API for user-facing APIs
- `gRPC-Web` for browser; but not recommended for communication with browser
	- HTTP/2 requirement
	- streaming not supported
	- latency because of proxy
	- Cross Origin Resource Sharing (CORS) settings
	- use [[SignalR]] for HTTP 1.1 (uses JSON serialization)

## See also

- [Protocol Buffers](https://github.com/protocolbuffers/protobuf)