# `Box<T>`
- `Box<T>` owns its data singularly

## Methods
- `impl<T> Box<T>`
    - `new (x: T) -> Box<T>`

## Example
```rust
// has infinite size:
enum List {
    Cons(i32, List),
    Nil,
}
//has known size:
enum List {
    Cons(i32, Box<List>),
    Nil,
}
let list = Cons( 1, Box::new( Cons( 2, Box::new(3, Nil) ) ) );
```

# `Rc<T>`
- `Rc<T>` allows for multiple ownership
- Rc uses reference counting

## Methods
- `impl<T> Rc<T>`
    - `new(x: T) -> Rc<T>`
    - `Rc::clone(x: &T) -> Rc<T>`
    - `Rc::strong_count(x: &Rc<T>)`

## Examples
```rust
enum List {
    Cons(i32, Rc<List>),
    Nil,
}
let a = Rc::new( Cons(5, Rc::new( Cons(10, Rc::new(Nil) ) )) );
let b = Cons(3, Rc::clone(&a));
let c = Cons(4, Rc::clone(&a));
```
