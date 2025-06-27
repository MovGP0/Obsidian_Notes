```typescript
type Text = string | string[];
```

```typescript
const data: string | string[] = getData();
```

```typescript
type Size = "small" | "medium" | "large";
```

## Compound unions

```typescript
type Responses = 
    | { status: 200, data: any }
    | { status: 301, to: string }
    | { status: 400, error: Error }
    // ...
```

```typescript
switch(response.status) {
    case 200: // ...
    case 301: // ...
    case 400: // ...
    // ...
    default: // ...
}
```

## Template unions

```typescript
type SupportedLanguage = "en" | "de";
type ElementType = "header" | "footer";

type ElementLanguage = `${ElementType}_${SupportedLanguage}`;
```
is the same as
```typescript
type ElementLanguage = "header_en" | "header_de" | "footer_en" | "footer_de";
```


