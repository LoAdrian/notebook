|           | mutable borrows | thread safe | multiple ownership | runtime checked borrows |
| :---      | :---:           | :---:       | :---:              | :---:                   |
| Box       | x               |             |                    |                         |
| Rc        |                 |             | x                  |                         |
| RefCell   | x               |             |                    | x                       |




# `Box<T>`
- `Box<T>` owns its data singularly

## API
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

## API
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

# `RefCell<T>`
- Uses interior mutability
- Borrowing rules are enforced at runtime 
- Represents single ownership over the data it holds.
- Used when sure that that borrowing rules are followed, but the compiler cannot check this.

## API
- `impl<T> RefCell<T>`
    - `new(x: T) -> RefCel<T>`
    - `borrow(&self) -> Ref<'_, T>`
    - `try_borrow(&self) -> Result<Ref<'_, T>, BorrowError>`
    - `borrow_mut(&self) -> RefMut<'_, T>`
    - `try_borrow_mut(&self) -> Result<RefMut<'_, T>, BorrowError>`
    - `get_mut(&mut self) -> &mut T`

## Examples
