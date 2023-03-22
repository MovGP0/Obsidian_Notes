| Algorithm              | Mechanism                                                   | Pros                                                       | Cons                                                      |
| ---------------------- | ----------------------------------------------------------- | ---------------------------------------------------------- | --------------------------------------------------------- |
| Token Bucket           | Tokens added to bucket at a fixed rate, removed on request  | Smooth rate-limiting; Burst allowance; Easy implementation | Token accumulation can lead to large bursts               |
| Leaking Bucket         | Fixed-size buffer; Drips out packets at a constant rate     | Smooth rate-limiting; Predictable output rate              | Less flexible; Discards packets if bucket overflows       |
| Fixed Window Counter   | Count requests in a fixed time window; Reset after window   | Simple implementation; Low overhead                        | Susceptible to burst traffic at window edges              |
| Sliding Window Log     | Log of request timestamps; Check if within time window      | Accurate rate-limiting; No bursts; Handles high throughput | Higher memory usage; More complex implementation          |
| Sliding Window Counter | Count requests in time slots; Slide window for new requests | Better burst control than fixed window; Low memory usage   | More complex implementation; Less accurate than log-based |

- Hard Rate-Limiting: Limits cannot be exceeded
- Soft Rate-Limiting: Limits can be exceeded for a short duration