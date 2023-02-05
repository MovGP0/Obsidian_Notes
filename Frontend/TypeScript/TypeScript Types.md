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

## Type Literals

| Type       | Example                       | Description |
| ---------- | ----------------------------- | ----------- |
| `Object`   | `{ field: string }`           |             |
| `Function` | `(arg: number) => string`     |             |
| `Array`    | `string[]` or `Array<string>` |             |
| `Tuple`    | `[string, number]`            |             |

## Algebraic Type

```typescript
number | string
```
