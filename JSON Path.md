| Expression        | Description                                                                                                                |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------- |
| `$`               | The root object or array                                                                                                   |
| `.PROPERTY`       | Selects the specified property in a parent object                                                                          |
| `['PROPERTY']`    | Selects the specified property in a parent object. Uses single-quotes.                                                     |
| `[5]`             | Selects the `5`th element from an array. Indexes are 0-based.                                                              |
| `[1,3,5,…]`       | Selects array elements with the specified indices                                                                          |
| `..PROPERTY`      | Searches property by name recursively                                                                                      |
| `*`               | Wildcard selects all elements in an object or an array. Ie. all properties `address.*` or all elements in array `book[*]`. |
| `[0:10]`          | Selects array elements from the `0` index and up to, but not including, `10` index.                                        |
| `[:10]`           | Selects the first `10` elements of the array.                                                                              |
| `[10:]`           | Selects the 11th element till the end                                                                                      |
| `[-10:]`          | Selects the last `10` elements of the array.                                                                               |
| `[?(EXPRESSION)]` | Filter expression                                                                                                          |
| `[(EXPRESSION)]`  | Script expressions can be used instead of explicit property names or indexes.                                              |
| `@`               | Used in filter expressions to refer to the current node being processed.                                                   |

Notes:
- JSONPath expressions, including property names and values, are **case-sensitive**.
- Unlike XPath, JSONPath does not have operations for accessing parent or sibling nodes from the given node.

## JSON Path Filters

| Operator      | Description                                                                                     | Examples                                                                                         |
| ------------- | ----------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| `==`          | Equals to. Uses single quotes.                                                                  | `[?(@.color=='red')]`                                                                            |
| `!=`          | Not equal to. Uses single quotes.                                                               | `[?(@.color!='red')]`                                                                            |
| `>`           | Greater than                                                                                    |                                                                                                  |
| `>=`          | Greater than or equal to                                                                        |                                                                                                  |
| `<`           | Less than                                                                                       |                                                                                                  |
| `<=`          | Less than or equal to                                                                           |                                                                                                  |
| `=~`          | JavaScript regular expression.                                                                  | ```[?(@.description =~ /cat.*/i)]```                                                             |
| `!`           | Used to negate a filter.                                                                        | `[?(!@.isbn)]`                                                                                   |
| `&&`          | Logical AND, used to combine multiple filter expressions.                                       | ```[?(@.category=='fiction' && @.price < 10)]```                                                 |
| `||`          | Logical OR, used to combine multiple filter expressions.                                        | ```[?(@.category=='fiction' || @.price < 10)]```                                                 |
| `in`          | Checks if the left-side value is present in the right-side list.                                | `[?(@.size in ['M', 'L'])]`, `[?('S' in @.sizes)]`                                               |
| `nin`         | Opposite of `in`.                                                                               | `[?(@.size nin ['M', 'L'])]`, `[?('S' nin @.sizes)]`                                             |
| `subsetof`    | Checks if the left-side array is a subset of the right-side array.                              | `[?(@.sizes subsetof ['M', 'L'])]`, `[?(['M', 'L'] subsetof @.sizes)]`                           |
| `contains`    | Checks if a string contains the specified substring, or an array contains the specified element | `[?(@.name contains 'Alex')]`, `[?(@.numbers contains 7)]`, `[?('ABCDEF' contains @.character)]` |
| `size`        | Checks if an array or string has the specified length.                                          | `[?(@.name size 4)]`                                                                             |
| `empty true`  | Matches an empty array or string                                                                | `[?(@.name empty true)]`                                                                         |
| `empty false` | Matches a non-empty array or string.                                                            | `[?(@.name empty false)]`                                                                        |
