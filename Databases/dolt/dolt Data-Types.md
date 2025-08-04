---
title: Data-Types
---
## Numeric Data Types

| Data Type      | Size / Range                                                                     | Description / Use-case                           |
| -------------- | -------------------------------------------------------------------------------- | ------------------------------------------------ |
| `TINYINT`      | 1 byte; -128 to 127 (signed), 0 to 255 (unsigned)                                | Small integers (e.g., age, status codes)         |
| `SMALLINT`     | 2 bytes; -32,768 to 32,767 (signed), 0 to 65,535 (unsigned)                      | Counters, smaller-range integers                 |
| `MEDIUMINT`    | 3 bytes; -8,388,608 to 8,388,607 (signed), 0 to 16,777,215 (unsigned)            | Intermediate-range integers                      |
| `INT`          | 4 bytes; -2,147,483,648 to 2,147,483,647 (signed), 0 to 4,294,967,295 (unsigned) | General-purpose integers (e.g., IDs)             |
| `BIGINT`       | 8 bytes; -9.22e18 to 9.22e18 (signed), 0 to 1.84e19 (unsigned)                   | Large-range integers, high-precision counters    |
| `DECIMAL(M,D)` | Up to 65 digits total; storage varies by precision                               | Exact fixed-point (e.g., financial calculations) |
| `FLOAT`        | 4 bytes; single-precision approximation                                          | Approximate numeric (scientific calculations)    |
| `DOUBLE`       | 8 bytes; double-precision approximation                                          | High-precision approximate numeric               |
| `BIT(M)`       | M bits (1–64)                                                                    | Bit-field storage (flags, Boolean sets)          |

## Date and Time Data Types

| Data Type     | Range / Storage                                                           | Description / Use-case                                                               |
| ------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| `DATE`        | 3 bytes; `'1000-01-01'` to `'9999-12-31'`                                 | Calendar dates                                                                       |
| `DATETIME`    | 5 bytes + fsp; `'1000-01-01 00:00:00'` to `'9999-12-31 23:59:59'`         | Date and time without time zone<br>⚠️ Always store datetimes as UTC in the database  |
| ⚠️`TIMESTAMP` | 4 bytes + fsp; `'1970-01-01 00:00:01'` UTC to `'2038-01-19 03:14:07'` UTC | Date and time with automatic time-zone conversion<br>⚠️ Avoid because of Y2K38 issue |
| `TIME`        | 3 bytes + fsp; `'-838:59:59'` to `'838:59:59'`                            | Time of day or elapsed intervals                                                     |
| `YEAR`        | 1 byte; `1901` to `2155`, and `0000`                                      | Year values                                                                          |

### String Data Types

| Data Type    | Size / Storage                                    | Description / Use-case                        |
| ------------ | ------------------------------------------------- | --------------------------------------------- |
| `CHAR(n)`    | n bytes (0–255)                                   | Fixed-length strings (ISO codes, fixed codes) |
| `VARCHAR(n)` | up to 65,535 bytes (varies by row size & charset) | Variable-length strings (names, titles)       |

#### Text Data Types

| Data Type    | Size / Storage            | Description / Use-case          |
| ------------ | ------------------------- | ------------------------------- |
| `TINYTEXT`   | up to 255 bytes           | Small text (short descriptions) |
| `TEXT`       | up to 65,535 bytes        | Text data (comments)            |
| `MEDIUMTEXT` | up to 16,777,215 bytes    | Medium text (articles)          |
| `LONGTEXT`   | up to 4,294,967,295 bytes | Large text (logs, content)      |

### Binary/BLOB Data Types

| Data Type      | Size / Storage            | Description / Use-case             |
| -------------- | ------------------------- | ---------------------------------- |
| `BINARY(n)`    | n bytes (0–255)           | Fixed-length binary data           |
| `VARBINARY(n)` | up to 65,535 bytes        | Variable-length binary data        |
| `TINYBLOB`     | up to 255 bytes           | Small binary objects (thumbnails)  |
| `BLOB`         | up to 65,535 bytes        | Binary objects (documents)         |
| `MEDIUMBLOB`   | up to 16,777,215 bytes    | Medium binary objects              |
| `LONGBLOB`     | up to 4,294,967,295 bytes | Large binary objects (media files) |

### Spatial Data Types

| Data Type            | Storage                                      | Description / Use-case                                                |
| -------------------- | -------------------------------------------- | --------------------------------------------------------------------- |
| `GEOMETRY`           | 4 bytes SRID + WKB (variable)                | Any geometry type; good for polymorphic columns                       |
| `POINT`              | 4 bytes SRID + WKB (≈21 bytes for one point) | Single coordinate pair (e.g. GPS location)                            |
| `LINESTRING`         | 4 bytes SRID + WKB (1 + 4 + 16×N coords)     | Sequence of points (e.g. roads, paths)                                |
| `POLYGON`            | 4 bytes SRID + WKB (includes ring data)      | Closed shapes (e.g. country or district boundaries)                   |
| `MULTIPOINT`         | 4 bytes SRID + WKB (collection of points)    | Multiple discrete locations (e.g. delivery stops)                     |
| `MULTILINESTRING`    | 4 bytes SRID + WKB (collection of lines)     | Multiple paths grouped (e.g. network of routes)                       |
| `MULTIPOLYGON`       | 4 bytes SRID + WKB (collection of polygons)  | Multiple areas (e.g. archipelagos, administrative regions)            |
| `GEOMETRYCOLLECTION` | 4 bytes SRID + WKB (mixed types)             | Heterogeneous collections (e.g. a park with walking trails and lakes) |

### JSON

> [!Note]
> JSON can be indexed via generated columns.
> JSON paths can be used for querying.

| Data Type | Size / Storage                                       | Description / Use-case                                                                            |
| --------- | ---------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| `JSON`    | up to `max_allowed_packet` (default ~64MB, max ~4GB) | Store and query semi-structured JSON documents<br>use JSON functions and generated-column indexes |

### Other

| Data Type | Size / Storage                   | Description / Use-case                       |
| --------- | -------------------------------- | -------------------------------------------- |
| `ENUM`    | up to 65,535 distinct values     | Enumeration lists (status codes, categories) |
| `SET`     | up to 64 members; stored as bits | Bit-mapped lists (tags, flags)               |

### XML

> [!Note]
> There is no `XML` column type.
> XML data is stored using [[#Text Data Types]].

There are functions for handling of XML data. See [MySQL Developer Zone: XML Functions](https://dev.mysql.com/doc/en/xml-functions.html) for details.

| Function                                     | Purpose                                                                                   |
| -------------------------------------------- | ----------------------------------------------------------------------------------------- |
| `ExtractValue(xml_frag, xpath_expr)`         | Returns the text (`CDATA`) of the first text node matching the XPath expression           |
| `UpdateXML(xml_target, xpath_expr, new_xml)` | Replaces the fragment matching `xpath_expr` in `xml_target` and returns the modified XML) |

```sql
-- Store an XML document
CREATE TABLE configs (
  id INT PRIMARY KEY,
  settings LONGTEXT
);

INSERT INTO configs
VALUES (1, '<config><mode>production</mode></config>');

-- Extract a value using XPath
SELECT ExtractValue(settings, '/config/mode')
  FROM configs
  WHERE id = 1;

-- Update the XML fragment
UPDATE configs
  SET settings = UpdateXML(
    settings,
    '/config/mode',
    '<mode>development</mode>'
  )
  WHERE id = 1;
```
