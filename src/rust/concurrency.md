# Traits
## `Send`
- Is implemented on any type that is safe to send to another thread.
- `T` is automatically `Send` if all its fields are `Send`
- `T` needs to be `Send` to be contained in a `Mutex`

## `Sync`
- Is implemented on any type that is safe to be shared between threads.
- `T` is `Sync` if `&T` is `Send`
- `T` needs to be `Sync` to be refered to by an `Arc`

# Pointers
## `Mutex`
- `Mutex` is a threadsafe version of `RefCell`
- `Mutex` implements the interior mutability pattern.

## `Arc`
- `Arc` is a threadsafe version of `Rc`
- `Arc` is an immutable, shared reference that shares ownership to a common value.

# Message Passing
> Do not communicate by sharing memory. Share memory by communicating.

```rust
use std::sync::mpsc;
use std::thread;

fn main() {
    let (tx, rx) = mpsc::channel();

    thread::spawn(move || {
        let val = String::from("hello");
        //sending ownership of val
        tx.send(val).unwrap(); //panic in case of error
    });
    let received = rx.recv().unwrap(); //panic in case of error
    println!("{}", received); //prints: "hello"
}
```

## Multiple Messages and Transmitters
```rust
let (tx, rx) = mpsc::channel();

let tx1 = tx.clone();
thread::spawn(move || {
    let vals = vec![
        String::from("hi"),
        String::from("from"),
        String::from("the"),
        String::from("thread"),
    ];

    for val in vals {
        tx1.send(val).unwrap();
        thread::sleep(Duration::from_secs(1));
    }
});

thread::spawn(move || {
    let vals = vec![
        String::from("more"),
        String::from("messages"),
        String::from("for"),
        String::from("you"),
    ];

    for val in vals {
        tx.send(val).unwrap();
        thread::sleep(Duration::from_secs(1));
    }
});

for received in rx {
    println!("Got: {}", received);
}
```