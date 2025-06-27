## Base architecture

- Use a CDN for static files

- Use a Load Balancers before Web and Backend-Services
	- Web Services shoud be state-free
	- Use a (distributed) caching layer for state

- Use Database replication for relational database servers

- Use NoSQL where applyable
	- Use document databases
	- Use search databases to find text documents
	- Use graph databases to find nodes via connections (ie. contacts of contacts)

- Use Message Queues for async processing
	- consider CQRS architectures

- Use a distributed logging solution

- Use a distributed metrics solution

- Use Retry-Policies
	- Don't make retry time too short

- Use [[Rate-Limiting Algorithms]]

- Use [[Consistent Hashing]] to place data in the proper location

- Use a centralized Unique-Key generator
	- UUID/GUIDs are easy solution
	- Central Ticket server works well, but is a single-point-of-failure
	- Consider [[Twitter Snowflake]]

- Use a URL shortening service and [HashID](https://hashids.org/net/)

- Use a [[Bloom Filter]] to check if a value is likely contained in a set of values
