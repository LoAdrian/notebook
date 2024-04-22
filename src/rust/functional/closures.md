# Closures
```rust
fn add_one_v1       (x: u32) -> u32     { x + 1 }
let add_one_v2 =    |x: u32| -> u32     { x + 1 };
let add_one_v3 =    |x|                 { x + 1 };
let add_one_v4 =    |x|                 x + 1;
```
## Captures
- Closures capture their environment mutably and/or immutably by borrowing or moving ownership.
- Moving ownership to the capture may be enforced with `move`: `move || println!("{}", x) //x is moved`


## Closure `Fn` Traits
- Closure Types are anonymous but closures implement 1, 2 or 3 Traits
- Closures borrow values or take ownership to them at the place where the closure is declared.
- The code inside the closure determines, what values are mutated and what values are moved out of the closure.

The three traits `FnOnce`, `FnMut` and `Fn` mark three nested classes of closures that are nested and defined as follows:

```
-----------------------
|       FnOnce        | <- Closure can be called at least once, may move captured values out of it's body and may mutate captured values
| ------------------- |
| |     FnMut       | | <- Closures that may mutate values but won't move captured values out of their body
| | --------------- | |
| | |    Fn       | | | <- Closures that do not mutate values and do not move captured values out of their body
| | --------------- | |
| ------------------- |
-----------------------
```
- `FnOnce` closures can be called at least once
- `FnMut` closures can be called more than once but capture their environment mutably
- `Fn` closures caputre their environment immutably or not-at-all

# Function Pointers
```rust
fn add(x: i32, y: i32) -> i32 {
    x + y
}

let add_v1: fn(i32, i32) -> i32 = add;

type Adder = fn(i32, i32) -> i32;
let add_v2: Adder = add;
```