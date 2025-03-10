
| Aspect                    | Class                                                               | Record                                                                                             | Struct                                                                                   | Ref struct                                                                                |
| ------------------------- | ------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| **Type Category**         | Reference type                                                      | Reference type (by default; can also be record struct)                                             | Value type                                                                               | Value type (with stack-only restrictions)                                                 |
| **Memory Allocation**     | Allocated on the heap                                               | Allocated on the heap                                                                              | Typically allocated on the stack or inline within containers                             | Must be allocated on the stack; cannot live on the heap                                   |
| **Mutability**            | Mutable by default (can be made immutable)                          | Immutable by default (though mutable records are possible)                                         | Often mutable, but best practice is to use immutable small structs                       | Typically mutable; designed for short-lived, stack-bound usage                            |
| **Equality Semantics**    | Uses reference equality unless overridden                           | Provides built-in value-based equality                                                             | Value equality (field-by-field, unless overridden)                                       | Value equality; but mainly used in performance-critical scenarios                         |
| **Inheritance**           | Supports inheritance and polymorphism                               | Supports inheritance (though often used in sealed hierarchies for simplicity)                      | No inheritance (only implements interfaces)                                              | No inheritance; cannot be boxed or used in certain polymorphic patterns                   |
| **Boxing/Unboxing**       | Not applicable                                                      | Not applicable                                                                                     | Can be boxed, which may affect performance                                               | Cannot be boxed, ensuring high performance and safety                                     |
| **Recommended Use Cases** | Complex objects with mutable state, rich behavior, and polymorphism | Immutable data models, DTOs, and scenarios benefiting from concise syntax and value-based equality | Small, frequently created data types (e.g., points, colors) where copying is inexpensive | Performance-critical, low-level operations (e.g., working with spans or unmanaged memory) |

### Recommendations

- **Class:**  
    Use classes when you need reference semantics, inheritance, polymorphism, or when managing complex, mutable objects.
    Ideal for domain models, entities, or any scenario where the overhead of heap allocation is acceptable.

- **Record:**  
    Records are a great choice for immutable data models and data transfer objects (DTOs). Their built-in value-based equality and succinct syntax make them well-suited for scenarios where you want to compare object values rather than references.

- **Struct:**  
    Use structs for small, lightweight data types where value semantics (copy-by-value) are desirable. They’re especially useful when performance is critical and the overhead of heap allocation is to be avoided. However, be cautious with mutable structs to prevent unexpected behavior due to copying.

- **Ref struct:**  
    Ref structs are specialized value types designed to be stack-only. They are ideal when you need to work with temporary data, such as buffers or spans (e.g., `Span<T>`), that must not be moved to the heap for performance and safety reasons. Their restrictions (e.g., no boxing, no async/await) help enforce safe usage patterns for low-level memory access.
