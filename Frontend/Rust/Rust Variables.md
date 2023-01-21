```rust
let x = 5;

```

## Borrowing

```rust
let mut foobar = String::from("foobar");
let ref1 = &foobar; // ownership is given to ref1
let ref2 = &foobar; // ownership is given to ref2
```

```rust
let mut foobar = String::from("foobar");
let ref1 = &foobar; // ownership is given to ref1
let ref2 = &mut foobar; // does not work, since ref1 has ownership
```

```rust
let mut foobar = String::from("foobar");
{
    let ref1 = &foobar; // ownership is given to ref1
} // ownership is given back to foobar, since ref1 goes out-of-scope

let ref2 = &mut foobar; // ownership is given to ref2
```
