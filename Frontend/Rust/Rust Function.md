```rust
fn add(x: i32, y: u8) -> i32
{
    let iy:i32 = <i32>::from(y);
    return x + iy;
}

fn div(divisor: f32, dividend: f32) -> (f32, f32)
{
    // ...
    return (result, rest);
}
```
