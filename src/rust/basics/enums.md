# Defining Enums
```rust
enum Message<T> {
    Quit,
    Move {x: i32, y: i32},
    Write(String),
    ChangeColor(i32, i32, i32),
    CustomMessage(T),
}
impl<T> Message<T> {
    pub fn call(&self) {
        //body
    }
}
```
# `Option<T>`
```rust
enum Option<T> {
    None,
    Some(T),
}
```

# Matching
```rust
let optional: Option<i32> = get_i32_option();
let x: i32 = match optional {
    Some(x) => x,
    None => 0,
}
```
