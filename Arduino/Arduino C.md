## Data types in C

| **C Data Type**       | **C# Data Type**       | **Size (bytes)**   | **Remarks**                                                                   |
| --------------------- | ---------------------- | ------------------ | ----------------------------------------------------------------------------- |
| `char`                | `char`                 | 1 (C), 2 (C#)      | C's `char` is a single byte; in C#<br>`char` is Unicode and 2 bytes (UTF-16). |
| `signed char`         | `sbyte`                | 1                  | Represents signed 8-bit integers.                                             |
| `unsigned char`       | `byte`                 | 1                  | Represents unsigned 8-bit integers.                                           |
| `short` / `short int` | `short`                | 2                  | Represents signed 16-bit integers.                                            |
| `unsigned short`      | `ushort`               | 2                  | Represents unsigned 16-bit integers.                                          |
| `int`                 | `int`                  | 4                  | Represents signed 32-bit integers.                                            |
| `unsigned int`        | `uint`                 | 4                  | Represents unsigned 32-bit integers.                                          |
| `long`                | `long`                 | 4 (C), 8 (C#)      | C's `long` is platform-dependent, often 4 bytes on 32-bit systems.            |
| `unsigned long`       | `ulong`                | 4 (C), 8 (C#)      | Represents unsigned 64-bit integers in C#, platform-dependent in C.           |
| `long long`           | `long`                 | 8                  | In C#, `long` is 64-bit; equivalent to C's `long long`.                       |
| `unsigned long long`  | `ulong`                | 8                  | Represents unsigned 64-bit integers.                                          |
| `float`               | `float`                | 4                  | Represents single-precision floating-point numbers.                           |
| `double`              | `double`               | 8                  | Represents double-precision floating-point numbers.                           |
| `long double`         | (No direct equivalent) | 8 or 16            | Platform-dependent; use `System.Decimal` or `BigFloat` for precision.         |
| `_Bool` (C99)         | `bool`                 | 1                  | Boolean type: `true` or `false`.                                              |
| `void`                | `void`                 | N/A                | Represents "no type" in both languages.                                       |
| `size_t`              | `UIntPtr`              | Platform-dependent | Used for memory sizes and indexing; matches pointer size.                     |
| `ptrdiff_t`           | `IntPtr`               | Platform-dependent | Represents pointer differences.                                               |

**Notes**
- **Platform Dependency**: In C, sizes of types like `int`, `long`, and `long double` can vary depending on the platform. In C#, sizes are consistent across platforms.
- **Unsigned Types**: While C has unsigned integer types (`unsigned int`, `unsigned long`, etc.), C# explicitly includes `uint`, `ushort`, and `ulong`.
- **Boolean**: C uses `_Bool` (introduced in C99) or integers (`0` for false, non-zero for true). C# uses `bool`, which is stricter and only allows `true` or `false`.
- **Strings**: C represents strings as arrays of `char` ending with `\0`. C# has the `string` type, which is Unicode-based and immutable.

## Documenting a method in C

Can be done using [Doxygen](https://www.doxygen.nl/) syntax:
```c
/*!
 * \brief Initializes a Foo object with the specified operation mode.
 *
 * The "read" mode means one thing, while "write" mode means another.
 *
 * \param[in] mode OpMode enum value specifying initialization mode.
 *                 Valid options are ReadMode and WriteMode.
 *                 Any other value generates a warning message and returns false.
 * \return bool indicating success or failure of initialization.
 *         Returns true on success, false on failure.
 */
bool initMode(OpMode mode);
```

## Unit testing

Unit testing can be done using [ArduinoUnit](https://github.com/mmurdoch/arduinounit/)

```c
#include <ArduinoUnit.h>

test(ok) {
    int x = 3;
    int y = 3;
    assertEqual(x, y);
}

test(bad) {
    int x = 3;
    int y = 4; // This will fail
    assertNotEqual(x, y);
}

void setup() {
    Serial.begin(9600);
    while (!Serial) {}
}

void loop() {
    Test::run();
}
```
