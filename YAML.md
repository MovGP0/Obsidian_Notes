## YAML Basics

Comments are prefixed with `#`. Everything from the `#` till the line ending is ignored by the YAML interpreter.

Intendation must be done with spaces. Tabs are invalid.

Key-value pairs are separated with `:`
```yaml
config:
  key1: value1
  key2: value2
  key3: value3
```

Dictionaries can also be declared using `{,,}`
```yaml
config: {key1: value1, key2: value2, key3: value3}
```

List items are prefixed using `-`
```yaml
colors:
   - red
   - blue
   - green
```

Lists can also be declared using `[,,]`
```yaml
colors: [red, blue, green]
```

YAML syntax Version `%YAML 1.2` can precede documents. Multiple YAML documents in a file are separated using `---` at the beginning and `...` at the end
```yaml
%YAML 1.2
---
jane: "foo"
...
---
doe: "bar"
...
```

### Data types

#### Numeric data types

```yaml
foo: 5 # decimal integer
foo: 0x12d4 # hexadecimal integer
foo: 023332 # octal integer
foo: 3.14159 # float / fixed decimals
foo: 12.3015e+05 # float / with exponents
foo: .inf # float / positive infinity
foo: -.Inf # float / negative infinity
foo: .NAN # float / not a number
```

#### Strings

```yaml
foo: this is a test \n # string / '\' and 'n' are two separate characters
foo: "this is a test \n" # string / '\n' is the newline character

# multiline string / not preserving line ending
foo: >
    this is a
    multiline string
    
# multiline string / not preserving line ending / preserve whitespace
foo: >+
    this is a
    multiline string

# multiline string / preserving line ending
foo: | 
    this is a
    multiline string

# multiline string / preserve line ending / remove whitespace
foo: |-
    this is a
    multiline string
```

#### Booleans
```yaml
True, Yes, On  # represent TRUE
False, No, Off # represent FALSE
```

#### Null

Null value is represented by either `~` or `null`.
```yaml
foo: ~
bar: null
```

## See also
- [The Official YAML Web Site](https://yaml.org/)
- [YAML Tutorial](https://cloudbees.com/blog/yaml-tutorial-everything-you-need-get-started)