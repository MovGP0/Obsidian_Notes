## Declare Interface

```typescript
interface JSONResponse extends Response, HTTPAble {
    // public attribute
    version: number;

    payloadSize: number;

    outOfStock?: boolean;

    // two different versions to declare a function
    update: (retryTimes: number) => void;
    update(retryTimes: number): void;

    // callable interface
    (): JSONResponse;

    // callable interface overload
    (matcher: boolean): string;
    (matcher: string): boolean;

    // new(value) constructor
    new(value: string): JSONResponse;

    // indexer
    [key: number]: number;

    // read-only property
    readonly body: string;

    // getter/setter
    get size(): number;
    set size(value: number | string);
}
```

## Generics

```typescript
interface APICall<Response> {
    data: Response
}
```

Usage:
```typescript
const api:APICall<PersonResponse> = /* ... */;
api.data
```

### Generic Constraints

```typescript
interface APICall<Response extends { id: number }> {
    data: Response;
}
```

Usage:
```typescript
const api:APICall<PersonResponse> = /* ... */;
api.data.id
```


## Interface Merging

When interface is defined with same name multiple times, the interface is merged into a single one.

```typescript
interface APICall {
     data: Response;
}

interface APICall {
     error?: Error;
}
```
is the same as
```typescript
interface APICall {
     data: Response;
     error?: Error;
}
```

## Implement Interface

```typescript
interface Syncable {
     // ...
}

class Account implements Syncable {
     // ...
}
```