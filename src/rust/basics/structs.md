# Basic Structs
```rust
struct StructName <'l, T> { // 'l being a generic lifetime parameter and T being a generic Type Parameter
    field1: Type1,
    field2: &'l Type2,
}

let s = StructName<Type> {
    field1: Type1::new(),
    field2: &value2,
}

let x = s.field1;
```

# Tuple Structs
```rust
struct Color(r: u32, g: u32, b: u32);
let black = Color(0, 0, 0);
```

# Unit-Like Structs

```rust
struct Unit;
let u = Unit;
let actualUnit = ();
```

# Methods
```rust
struct X {
    x: i32,
    y: u32,
}

impl X{
    // method
    pub fn get_x(&self) -> i32 {
        x
    }
    // method
    pub fn set_x(&mut self, value: i32) { // -> ()
        self.x = value;
    }
    // associated function
    pub fn new(x: i32, y: u32) -> Self {
        Self{
            x,
            y,
        }
    }
}
```