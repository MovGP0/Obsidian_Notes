## Inheritance

Inheritance is not supported. Use composition over inheritance.

## Encapsulation

Only make some objects public, while keeping others private.

Expose only some methods of the internal `Vec<T>` type:
```rust
pub struct CustomStack {
    elements: Vec<i32>,
    max: i32
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

