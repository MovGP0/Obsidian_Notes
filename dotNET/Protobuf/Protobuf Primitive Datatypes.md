- field values cannot be `null`
- all fields have a default value (ie. `0` for numbers and `""` for strings)

## List of Primitives
 
- `double`: double-precision floating-point number
- `float`: single-precision floating-point number
- `int32`: 32-bit integer
- `int64`: 64-bit integer
- `uint32`: 32-bit unsigned integer
- `uint64`: 64-bit unsigned integer
- `sint32`: signed int32, uses variable-length encoding
- `sint64`: signed int64, uses variable-length encoding
- `fixed32`: 32-bit unsigned integer (4 bytes)
- `fixed64`: 64-bit unsigned integer (8 bytes)
- `sfixed32`: signed fixed-point number (4 bytes)
- `sfixed64`: signed fixed-point number (8 bytes)
- `bool`: boolean
- `string`: UTF-8 encoded or 7-bit ASCII text
- `bytes`: sequence of bytes

## Nullable types

Nullable types can be represendet using the `oneof` keyword. 
Predefined versions are located in `Google.Protobuf.WellKnownTypes`:

- `google.protobuf.DoubleValue`
- `google.protobuf.FloatValue`
- `google.protobuf.Int32Value`
- `google.protobuf.Unt64Value`
- `google.protobuf.Uint32Value`
- `google.protobuf.Uint64Value`
- `google.protobuf.BoolValue`
- `google.protobuf.StringValue`
- `google.protobuf.BytesValue`

## Time and Duration

Duration and Timestamp are located in `Google.Protobuf.WellKnownTypes`:

- `google.protobuf.Duration`
- `google.protobuf.Timestamp`

## Special types

- `google.protobuf.Empty` empty message (Unit)
- `google.protobuf.Any` any data; serialized as byte array
- `google.protobuf.Value` any one of a given list of types

### Empty

```csharp
using Google.Protobuf.WellKnownTypes;

var empty = new Empty();
```

### Any

```csharp
using Google.Protobuf;
using Google.Protobuf.WellKnownTypes;

var entity = new Any
{
	TypeUrl = "type.googleapis.com/google.protobuf.StringValue",
	Value = ByteString.CopyFromUtf8("Hello, world!")
};
```

### Struct

```csharp
using Google.Protobuf.WellKnownTypes;

var entity = new Struct
{
	Fields =
	{
		["foo"] = new Value { StringValue = "Hello, world!" },
		["bar"] = new Value { NumberValue = 42 }
	}
};
```

### Value

Represents one of the following types:

- `NullValue`
- `NumberValue` represents a `double`
- `StringValue`
- `BoolValue`
- `ListValue` represents a collection of values
- `StructValue` represents a dictionary

```csharp
using Google.Protobuf.WellKnownTypes;

var entity = new Value  
{  
    BoolValue = false  
};  
  
switch (entity.KindCase)  
{
	case Value.KindOneofCase.BoolValue:
		Console.WriteLine(entity.BoolValue);
		break;

    case KindOneofCase.StringValue:
	    Console.WriteLine(entity.StringValue);
		break;

    // ...

	default:
		throw new NotSupportedException();
};
```

## See also

- [Protobuf scalar data types](https://learn.microsoft.com/en-us/dotnet/architecture/grpc-for-wcf-developers/protobuf-data-types)
- [Google Proto Files](https://github.com/protocolbuffers/protobuf/tree/main/src/google/protobuf)