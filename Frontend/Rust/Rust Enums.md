
## Declare enum

```rust
#[derive(Debug, Clone, Copy, PartialEq, Eq)]
enum Sex {
    Male,
    Female,
    Other,
    NotApplicable,
    Unknown,
    NonBinary,
    Ambiguous
}
```

Match operator:
```rust
impl Sex {
	fn get_sex_string(&self) -> &'static str { 
		return match &self { 
			AdministrativeSex::Male => "M",
			AdministrativeSex::Female => "F",
			AdministrativeSex::Other => "O",
			AdministrativeSex::Unknown => "U",
			AdministrativeSex::NotApplicable => "N",
			AdministrativeSex::Unknown => "U",
		    AdministrativeSex::NonBinary => "X",
		    AdministrativeSex::Ambiguous => "A",
		    _ => unreachable!()
		};
	}
}
```

Usage:
```rust
let sex = Sex::Male;
let sex_string = sex.get_sex_string();
```

## Enum with data

Enum values can have a data assinged.

Example with generic type:
```rust
enum Option<T> {
    Some(T),
    None
}
```

Usage:
```rust
let some = Option::Some(true);
let none = Option::None;
```
