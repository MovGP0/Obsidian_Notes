Distribute data among nodes in a balanced and consistent manner

| Algorithm                             | Mechanism                                                         | Pros                                                              | Cons                                                          |
| ------------------------------------- | ----------------------------------------------------------------- | ----------------------------------------------------------------- | ------------------------------------------------------------- |
| Basic Consistent Hashing              | Hash nodes and keys, assign keys to the nearest node clockwise    | Balanced load distribution; Minimal data movement on node changes | Non-uniform data distribution; Depends on the quality of hash |
| Consistent Hashing with Virtual Nodes | Map multiple virtual nodes per node; Assign keys to virtual nodes | Better load balancing; Improved fault tolerance                   | More complex implementation; Increased memory overhead        |
| Rendezvous (HRW) Hashing              | Hash node-key pairs; Assign key to node with the highest hash     | Load balancing; Minimal data movement; No virtual nodes           | Slower performance; Depends on the quality of hash function   |

