|           | mutable borrows | thread safe | multiple ownership | runtime checked borrows |
| :---      | :---:           | :---:       | :---:              | :---:                   |
| Box       | x               |             |                    |                         |
| Rc        |                 |             | x                  |                         |
| RefCell   | x               |             |                    | x                       |
| Arc       |                 | x           | x                  |                         |
| Mutex     | x               | x           |                    | x                       |




# `std::boxed::Box<T>`
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

# `std::rc::Rc<T>`
- `Rc<T>` allows for multiple ownership
- Rc uses reference counting

## API
- `impl<T> Rc<T>`
    - `new(x: T) -> Rc<T>`
    - `Rc::clone(&self) -> Rc<T>`
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

# `std::cell::RefCell<T>`
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
```rust
let x = Rc::new( RefCell::new(42) );
let y = Rc::clone(x);
let x_mut = x.borrow_mut();
*x_mut += 1337; // mutable access to a shared ownership value
```

# `std::sync::Arc<T>`
- Threadsafe version of Rc<T>`

## API
- `impl<T> Arc<T>`
    - `new(x: T) -> Arc<T>`
    - `Arc::clone(&self) -> Arc<T>`
    - `Arc::strong_count(x: &Arc<T>)`

## Examples

```rust
let counter = Arc::new(Mutex::new(0));
let mut handles = vec![];

for _ in 0..10 {
    let counter = Arc::clone(&counter);
    let handle = thread::spawn(move || {
        let mut num = counter.lock().unwrap();

        *num += 1;
    });
    handles.push(handle);
}

for handle in handles {
    handle.join().unwrap();
}

println!("Result: {}", *counter.lock().unwrap());
```

# `std::sync::Mutex<T>`
- Like `RefCell<T>` but threadsafe

## API
- `impl<T> Mutex<T>`
    - `pub const fn new(t: T) -> Mutex<T>`
    - `pub fn lock(&self) -> LockResult<MutexGuard<'_, T>>`

## Examples 
```Rust
let m = Mutex::new(5);

{
    let res = m.lock(); // LockResult (May be a PoisonError whenever a thread that holds the lock fails)
    let mut num = res.unwrap(); // MutexGuard (implements Deref and DerefMut)
}
```
