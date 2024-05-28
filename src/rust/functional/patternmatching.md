# Places for Patterns
## Match Arms
```rust
//Syntax
match VALUE {
    PATTERN => EXPRESSION,
    PATTERN => EXPRESSION,
}
//Example
match x {
    None => None,
    Some(i) => Some(i+1),
}
```

## `if let` Expressions
```rust
//Syntax
if let PATTERN = VALUE {
    //...
}
//Example
if let Some(x) = opt {
    return Some(x + 1);
}
```

## `while let` Expressions
```rust
//Syntax
while let PATTERN = VALUE {
    //...
}
//Example
while let Some(top) = stack.pop() {
    println!("{}", top);
}
```

## `for` Loops
```rust
//Syntax
for PATTERN in value {
    //...
}
//Example
for (index, value) in v.iter().enumerate() {
    println("{}:{}", index, value);
}
```

## `let` Statements
```rust
//Syntax
let PATTERN = VALUE;
//Example
let (x,y,z) = (1,2,3);
```

## Function Parameters
```rust
//Syntax
fn FN_NAME(PATTERN: TYPE) {}
//Example
fn print_coordinates(&(x,y): &(f32,f32)){
    println!("{}/{}", x, y);
}
```

# Refutability
Any Pattern is Irrefutable if it matches for any possible value without fail.
```rust
let x = 5;
//Let statements must only contain irrefutable values
```
Any Pattern is Refutable if it does not match for any possible value.
```rust
if let Ok(x) = opt {}
//Rust would only warn if we would use an irrefutable pattern here.
```

# Pattern Syntax

## Named Variable Patterns
Named irrefutable patterns that match any value like `if let Ok(x)`.

## Multiple Patterns with `|`
Matching multiple Patterns using `|`.
```rust
match x {
    1 | 2 => println!("x is one or two"),
}
```

## Matching Ranges of values with `..=`
```rust
match x {
    1..=5 => println!("one through five"),
    _ => println!("something else"),
}
match y {
    'a'..='z' => println!("lower case char"),
    _ => println!("something else"),
}
```

## Destructuring Structs
```rust
struct Point {
    x: i32,
    y: i32,
}
fn main() {
    let p = Point {x: 5, y: 6};
    let Point {x: a, y: b} = p;
    assert_eq!(5, a);
    assert_eq!(6, b);

    match p {
        Point {x, y: 0} => println!("x is {}, y is zero", x);
        Point {x, y} => println!("x is {}, y is {}", x, y);
    }
}
```

## Destructuring Enums 
```rust
enum Msg {
    Move { x: i32, y: i32 },
    ChangeColor(i32, i32, i32),
}

fn main() {
    let msg = Message::ChangeColor(0, 160, 255);

    match msg {
        Message::Move { x, y } => {
            println!("Move in the x direction {x} and in the y direction {y}");
        }
        Message::ChangeColor(r, g, b) => {
            println!("Change the color to red {r}, green {g}, and blue {b}",)
        }
    }
}
```

## Destructuring Nested Data
- Destructuring also works on nested values (a struct in a struct in an enum in a struct).

## Ignoring Values with `_`
```rust
fn foo(_: i32, y: i32) {}
match (x, y) {
    (Some(_), Some(_)) => /*...*/,
    _ => /*...*/,
}
```
## Ignoring an Unused Variable by starting with `_`
```rust
let _x = 5; //not yet used
if let Some(_s) {}
```

## Ignoring remaining parts of a value with `..`
```rust
let origin = Point {x: 1, y: 2, z: 3};
match origin {
    Point {first, ..} => /*...*/,
    Point {first, .., last} => /*..*/
}
```

## Match Guards
```rust
match num {
    Some(x) if x % 2 == 0 => /*..*/,
    Some(x) => /*..*/,
    None => /*..*/,
}
```

## @ Bindings

```rust
enum Message {
    Hello { id: i32 },
}

let msg = Message::Hello { id: 5 };

match msg {
    Message::Hello {
        id: id_variable @ 3..=7,
    } => println!("Found an id in range: {}", id_variable),
    Message::Hello { id: 10..=12 } => {
        println!("Found an id in another range")
    }
    Message::Hello { id } => println!("Found some other id: {}", id),
}
```
> Using @ lets us test a value and save it in a variable within one pattern.