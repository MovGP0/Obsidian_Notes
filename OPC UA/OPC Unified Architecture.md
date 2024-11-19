OPC UA (OLE for Process Control Unified Architecture) is a machine-to-machine communication protocol for industrial automation

- Methods are limited to 10 arguments; but arguments may be arrays and structures.
- The following protocols are defined
	- OPC UA over TCP
	- HTTPS
	- WebSockets
- The following serialization schemes are defined:
	- XML
	- UA Binary
	- JSON

## URI Scheme

`opc.tcp://127.0.0.1:4840`

## OPC UA Data Types

| Built-in Type   | C# Type           | Example                                                    | Details                                                 | NodeId Type     |
| --------------- | ----------------- | ---------------------------------------------------------- | ------------------------------------------------------- | --------------- |
| Boolean         | `bool`            |                                                            | 0 (false) or 1 (true)                                   | 0 (numeric)     |
| SByte           | `sbyte`           |                                                            | -128 to 127                                             |                 |
| Byte            | `byte`            |                                                            | 0 to 255                                                |                 |
| Int16           | `short`           |                                                            | -32,768 to 32,767                                       |                 |
| UInt16          | `ushort`          |                                                            | 0 to 65,535                                             |                 |
| Int32           | `int`             |                                                            | -2,147,483,648 to 2,147,483,647                         |                 |
| UInt32          | `uint`            |                                                            | 0 to 4,294,967,295                                      |                 |
| Int64           | `long`            |                                                            | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 |                 |
| UInt64          | `ulong`           |                                                            | 0 to 18,446,744,073,709,551,615                         |                 |
| Float           | `float`           |                                                            | IEEE single precision (32-bit) floating point value     |                 |
| Double          | `double`          |                                                            | IEEE double precision (64-bit) floating point value     |                 |
| StatusCode      | `uint`            |                                                            |                                                         |                 |
| String          | `string`          |                                                            |                                                         | 3 (string)      |
| DateTime        | `DateTime`        |                                                            | Number of 100-nanosecond intervals since 1/1/1601 (UTC) |                 |
| GUID            | `Guid`            |                                                            | 16-byte number used as a UUID                           | 4 (GUID)        |
| ByteString      | `byte[]`          |                                                            |                                                         | 5 (byte string) |
| XmlElement      | `XmlElement`      |                                                            |                                                         |                 |
| NodeId          | `NodeId`          | `{1, "TemperatureSensor1"}`                                | Namespace index and identifier                          |                 |
| ExpandedNodeId  | `ExpandedNodeId`  |                                                            | Similar to NodeId, includes namespace URI               |                 |
| QualifiedName   | `QualifiedName`   | `{1, "Temperature"}`                                       | Namespace index and string                              |                 |
| LocalizedText   | `LocalizedText`   | `{"en-US", "Temperature"}`                                 | String and a locale indicator                           |                 |
| NumericRange    | `string`          |                                                            | String (e.g., "0:4,1:5" for \[0..4\]\[1..5\] array)     |                 |
| Variant         | `object`          | `{Double, 23.5}`                                           | Can hold any built-in data type                         |                 |
| ExtensionObject | `ExtensionObject` |                                                            | Encapsulates complex data types                         |                 |
| DataValue       | `DataValue`       | `{23.5, Good, 2023-05-16T12:34:56Z, 2023-05-16T12:34:57Z}` | Composite of a value, timestamps, and status code       |                 |
| DiagnosticInfo  | `DiagnosticInfo`  |                                                            | Detailed error/diagnostic information                   |                 |

## Events

Custom data structure containing multiple fields that are sent from and to the machine.

```json
{
  "EventId": "12345",
  "EventType": "AlarmConditionType",
  "SourceNode": "1:TemperatureSensor1",
  "SourceName": "Temperature Sensor 1",
  "Time": "2023-05-16T12:34:56Z",
  "Message": "High temperature alarm",
  "Severity": 100,
  "AckedState": false,
  "ConfirmedState": false,
  "ActiveState": true,
  "ConditionName": "HighTemperature",
  "BranchId": "0",
  "Retain": true
}
```

## IP Stack

| Layer               | Protocol                                                                                | Description                                                               |
| ------------------- | --------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| Automation Protocol | [OPC UA](https://en.wikipedia.org/wiki/OPC_Unified_Architecture)                        |                                                                           |
| Application         | [MQTT](https://en.wikipedia.org/wiki/MQTT) (ISO/IEC 20922)                              | Message broker for message transmission                                   |
| Transport           | [TCP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol)                      | Protocol needs to support ordered, lossless, and bi-directional transport |
| Internet            | [IPv6](https://en.wikipedia.org/wiki/IPv6) / [IPv4](https://en.wikipedia.org/wiki/IPv4) |                                                                           |

## See also

- [OPC Foundation](https://opcfoundation.org/)
- [OPC OA Reference](https://reference.opcfoundation.org/)
  
## Software and Libraries

- [OPC UA Client](https://www.unified-automation.com/products/development-tools/uaexpert.html)
