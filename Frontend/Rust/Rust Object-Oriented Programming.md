## Inheritance

Inheritance is not supported. Use [composition over inheritance](https://en.wikipedia.org/wiki/Composition_over_inheritance).

## Encapsulation

Only make some objects public, while keeping others private.

Expose only some methods of the internal `Vec<T>` type:
```rust
pub struct CustomStack { // public struct
    elements: Vec<i32>, // private field
    max: i32 // private field
}

impl CustomStack {
    pub fn push(&mut self, elem: i32) {
        self.elements.push(elem);
        self.calculate_max();
    }

    pub fn pop(&mut self) -> Option<i32> {
        let el = self.elements.pop();
        match el {
            Some(x) => {
                self.calculate_max();
                Some(x);
            }
            None => None
        }
    }

    fn calculate_max(&mut self) {
        let m = self.elements.iter().max();
        match m {
	        None => self.max = 0,
	        Some(x) => self.max = *x
        }
    }

    pub fn new() {
        return CustomStack {
            elements: vec![],
            max: 0
        };
    }
}

fn main() {
    let mut cs = CustomStack::new();
    cs.push(5);
    cs.push(3);
    cs.push(10);
    cs.pop();
}
```

## Abstraction and Polymorphism

Implemented using [[Rust Generics and Traits]].

```rust
using std::f64::consts::{ PI };

trait Shape {
    fn area(&self) -> f64;
}

struct Rectangle {
    heigth: f64,
    width: f64
}

impl Shape for Rectangle {
    fn area(&shelf) -> f64 {
        self.heigth * self.width
    }
}

struct Triangle {
    a: f64,
    b: f64,
    c: f64
}

impl Shape for Triangle {
    fn area(&shelf) -> f64 {
        let (a, b, c) = (self.a, self.b, self.c);
        let s = (a + b + c) / 2.0;
        (s*(s-a)*(s-b)*(s-c)).sqrt()
    }
}

struct Circle {
    radius: f64
}

impl Shape for Circle {
    fn area(&self) -> f64 {
        PI * self.r * self.r
    }
}
```

## Static Dispatch / Monomorhpism

- Uses [[Rust Generics and Traits]]
- Converts generic code to fixed code
- Increases binary size, but very fast

```rust
fn print_area<T: Shape>(shape: T) {
    let area = shape.area();
    println!("area: {}", area);
}

fn main() {
    let shape = ...;
    print_area(shape);
}
```

## Dynamic Dispatch / Duck Typing

- Uses Trait Objects
- Does not work with Generics
- Smaller binary size, but slow

```rust
fn print_area(shape: &dyn Shape) {
    let area = shape.area();
    println!("area: {}", area);
}

fn main() {
    let shape = ...;
    print_area(&shape);
}
```
