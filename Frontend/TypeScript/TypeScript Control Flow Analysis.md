
## typeof (`===`)

Used for primitive data types
```typescript
const input: string | number = getUserInput();

if (input === "string") {
     // input: string
} else {
     // input: number
}
```

After narrowing the type, the properties can be accessed:
```typescript
const input: string | number = getUserInput();

if (input === "string" && input.length > 0) {
    // input: string
}
```

## `instanceof`

Used for classes
```typescript
const input: number | number[] = getUserInput();
if (input instanceof Array) {
    // input: number[]
} else{
    // input: number
}
```

## `in`

Check if property exists
```typescript
const input: string | { error: string } = getUserInput();
if ("error" in input) {
    logError(input.error);
} else{
    print(input);
}
```

## Type guard functions

Some types provide custom guard clauses
```typescript
const input: number | number[] = getUserInput();

if (Array.isArray(input)) {
    // input: number[] 
} else {
    // input: number
}
```

## Type guards

- `is` keyword to describe the type of the guard
- `instanceof`, `===`, and other checks are used to determine if it's the proper type

```typescript
function isError(obj: Response): obj is APIErrorResponse {
    return obj instanceof APIErrorResponse;
}
```

Usage:
```typescript
const response = getRensponse();

if (isError(response)) {
    // response: APIErrorResponse
}
```

## Assert functions

Throws an exception when assertion fails
```typescript
function assertResponse(obj: Response): asserts obj is APISuccessResponse {
    if(!(obj instanceof APISuccessResponse) {
        throw new Error("Response was not an APISuccessResponse");
    }
}
```

## `as const`

`as const` makes member properties immutable:

```typescript
const person = { firstName: "foo", lastName: "bar" } as const;
```

## See also

- [[TypeScript Discriminated Unions]]