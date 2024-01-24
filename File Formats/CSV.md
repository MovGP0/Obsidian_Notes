# Comma-Separated Values (CSV)

- Always uses `CRLF` for line separation.

## CSV Dialect Definition File (CSVDDF)

CSV Files can be described by an JSON file that describes the formatting rules when the CSV file was created.

```json
{
  "dialect": {
    "csvddfVersion": 1.2,
    "delimiter": ";",
    "doubleQuote": true,
    "lineTerminator": "\r\n",
    "quoteChar": "\"",
    "skipInitialSpace": true,
    "header": true,
    "commentChar": "#"
  }
}
```

## CSV On The Web (CSVW)

Standard how CSV Files should be formatted for web interchange.
Uses a JSON file for formatting definitions, as well as semantic informations.

## Whitespace-Separated Values (WSV)

- UTF-8 Unicode encoding
- `LF` character for line separation
- Whitespace for column separation
- `#` for comments
- Represents an Array of Arrays (Jagged Array); not limited to tables

**References**
- [WSV Guide](https://dev.stenway.com/WSV/Index.html)

## Alternatives

- [[YAML]]
- [Tom's Obvious Minimal Language (TOML)](https://toml.io/en/)
- [JavaScript Object Notation (JSON)](https://www.json.org/json-en.html), [HJSON](https://hjson.github.io/), [JSON5](https://json5.org/)

## References

- [RFC 4180 - Comma-Separated Values (CSV)](https://www.ietf.org/rfc/rfc4180.txt)
- [CSV Dialect](https://github.com/frictionlessdata/specs/tree/master/csv-dialect)
- [CSV Web](https://csvw.org/)
- [Whitespace-Separated Values (WSV)](https://www.whitespacesv.com/)
- 