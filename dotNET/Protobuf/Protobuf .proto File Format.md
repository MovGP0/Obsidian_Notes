## `.proto` File Format

Declare Metadata
```protobuf
// syntax declaration that specifies which 
// version of protobuf you are using (proto2 or proto3).
syntax = "proto3";

// package name that defines a namespace for your message types
// avoids name conflicts with other .proto files
package packagename;

// import other .proto files that you want to use in your file.
import "google/protobuf/timestamp.proto";
```

Set options
```protobuf
// options configure how the generated code behaves or looks like
option csharp_namespace = "Solution.Project";
option java_package = "com.companyname.packagename";
option java_outer_classname = "JavaClassName";
option go_package = "myprotobuf";
option optimize_for = SPEED; // SPEED, CODE_SIZE, LITE_RUNTIME
```

Declare Messages
```protobuf
// definitions that describe the fields and types of your data structures
// number defines the order in the serialization
message Person {
  string name = 1;
  int32 id = 2;
  string email = 3;

  // (untagged) enums
  enum PhoneType {
    MOBILE = 0;
    HOME = 1;
    WORK = 2;
  }

  message PhoneNumber {
    string number = 1;
    PhoneType type = 2;
  }

  repeated PhoneNumber phones = 4; // array of values

  google.protobuf.Timestamp last_updated = 5; // use imported datatype
}

message AddressBook {
  repeated Person people = 1;
}
```

Use `map<TKey, TValue>` for dictionaries
```protobuf
message Person {
    // ...
    map<string, PostalAddress> addresses = 6;
}
```

Use `oneof` for discriminated unions
```protobuf
message Person {
  oneof person {
    int32 id = 1;
    string name = 2;
  }
}
```

Declare Services
```protobuf
service ServiceName {
  rpc GetPerson(int32 personId) returns (Person);
  rpc SendPeople(stream Person) returns SuccessMessage;
  rpc GetPeople() returns (stream Person);
}
```

Deprecate fields
```protobuf
int32 some_old_field = 6 [deprecated=true];
```

## See also

- [[Protobuf Primitive Datatypes]]