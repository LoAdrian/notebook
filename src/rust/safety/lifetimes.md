# Syntax
## References
```rust
x: &'a str; //a string-slice with a lifetime denoted by 'a
```
## Functions
```rust
fn doSomething<'a, 'b>(x: &'a str, y: &'b str) -> &'a str {
  // a function taking a string slices and returning a slice with the same lifetime than the first parameter
}
fn doSomethingElse<'a>(x: &'a str, y: &'a str) -> &'a str {
  // the lifetime of the returned slice is at least as long as the shortest lifetime of the parameters
}
```
## Methods
```rust
impl<'a> S<'a> {
  fn hello(&'a str) -> &'a Self {
  }
}
```

## Structs
```rust
struct S<'a> {
  part: &'a str,
}

// instances of S cannot outlive their "part" member.
// s must live at least for 'a, where 'a is the greatest lifetime among annotated members
```

# Elision-Rules
1. Each reference-parameter is assigned an individual lifetime
2. If there is exectly one input parameter, its lifetime is assigned to all output
3. If any input-reference is `&self` or `&mut self` then the lifetime of said reference is assigned to the output

## Examples
```rust
fn first_word(s: &str) -> &str {
fn first_word<'a>(s: &'a str) -> &str { //after first rule
fn first_word<'a>(s: &'a str) -> &'a str { //after second rule
//third rule omitted because function is not a method

fn longest(x: &str, y: &str) -> &str {
fn longest<'a, 'b>(x: &'a str, y: &'b str) -> &str { //after first rule
//second rule cannot be applied because of ambiguous possibilities --> explicit lifetime parameters / annotations needed.
```

# Static Lifetime
```rust
let s: &'static str = "String literals are always static";
```
- `'static` denotes that a reference can live for the entire runtime of the program.
