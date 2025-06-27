## TypeScript Type Primitives

| Type        | Description                                              |
| ----------- | -------------------------------------------------------- |
| `boolean`   | boolean value, either `true` or `false`                  |
| `string`    | sequence of characters                                   |
| `number`    | numeric value                                            |
| `undefined` | absence of a value                                       |
| `null`      | deliberate non-value                                     |
| `any`       | any value and supersedes all other types                 |
| `unknown`   | any value but requires type checking before use          |
| `never`     | a value that will never occur                            |
| `void`      | absence of a return value                                |
| `bigint`    | big integer value                                        |
| `symbol`    | unique and immutable value that is used as an identifier |

## JavaScript Type Primitives

| Type      | Description                                                                    |
| --------- | ------------------------------------------------------------------------------ |
| `Date`    | specific moment in time                                                        |
| `Error`   | error object                                                                   |
| `Array`   | collection of values                                                           |
| `Map`     | collection of key-value pairs                                                  |
| `Set`     | collection of unique values                                                    |
| `Regexp`  | regular expression pattern                                                     |
| `Promise` | a value that may not be available yet, but will be at some point in the future |

## Custom types

### Primitive type

Provides an alias for a type
```typescript
type EmailAddress = string;
```

### Object

```typescript
const location: { x: number; y: number } = { x: 5, y: 7 };
```

```typescript
type Location {
    x: number;
    y: number;
}

const location: Location = { x: 5, y: 7 };
```

### Array

```typescript
let value: string[] = [1, 2, 3];
let value: Array<string> = [1, 2, 3];
let value: Array = /* ... */;
```

### Tuple

```typescript
const tuple: [string, number] = ["foo", 5];
```

```typescript
type MyTuple = [
    key: string,
    value: number
];

const tuple: MyTuple = ["foo", 5];
```

### Type intersection/merge

```typescript
type Location = { x: number } & { y: number };
```

### Type indexing

```typescript
type Response = { data: any };

type Data = Response["data"];
```

### `typeof`

```typescript
const type: Type = typeof data;
```

### `ReturnType`

Gets the return type of a function.

```typescript
const someFunction: Function = /* ... */;

type SomeFunctionReturnType = ReturnType<typeof someFunction>;

function test(fun: SomeFunctionReturnType) {
    // ...
}
```

### Type from Module

```typescript
const data: import("./data").data;
```

### Mapped type

```typescript
type Person = { firstName: string, lastName: string, age: number };

type Subscriber<Type> = {
    [Property in keyof Type]: (newValue: Type[Property]) => void
}

type PersonSubscriber = Subscriber<Person>;
```
results in 
```typescript
type PersonSubscriber = {
    firstName(newValue: string): void;
    lastName(newValue: string): void;
    age(newValue: number): void;
}
```

### Conditional types

```typescript
type HasFourLegs<Animal> = Animal extends { legs: 4 } ? Animal : Never;

type Animals = Bird | Dog | Ant | Wolf;

type FourLeggedAnimals = HasFourLegs<Animals>;
```

## See also

- [[TypeScript Discriminated Unions]]
