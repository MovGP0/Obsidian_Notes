## Sequence

Execute A then B.

```mermaid
flowchart LR

A --> B
```

## Parallel Split

Execute B and C after executing A.

```mermaid
flowchart LR

A --> B
A --> C
```

## Synchronization

Execute C when A and B have returned a value.

```mermaid
flowchart LR

A --> C
B --> C
```

## Exclusive Choice

Execute either B or C.

```mermaid
flowchart LR

A --> D{yes/no}
D -->|yes| B
D -->|no| C
```

## Simple Merge

Execute C when either A or B returns a value.

```mermaid
flowchart LR

A --> D{either}
B --> D
D --> C
```
