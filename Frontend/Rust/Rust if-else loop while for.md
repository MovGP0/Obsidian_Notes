## if-else
```rust
if x == 5 {
    // ...
}
else if x > 5 {
    // ...
}
else {
    // ...
}
```

## loop
```rust
fn main() {
    let mut x = 0;
    let result = loop {
        x += 1;
        if x==3 break;
    };
    println!("result = {}" x);
}
```

Loop can return result on break
```rust
fn main() {
    let mut x = 0;
    let result = loop {
        x += 1;
        if x==3 {
            break 2*x; // break gives return value
        }
    };
    println!("result = {}" x);
}
```

## while
```rust
fn main() {
  let mut x = 0;
  while x < 3 {
      x += 1;
      println!("x = {}", x);
  }
}
```

## for
```rust
fn main() {
    let arr = [10, 20, 30, 40];
    for item in arr.iter() {
        println!("item = {}", item);
    }
}
```
