# Literals

| kind | example |
| :--- | :---    |
| dec  | 42_1337 |
| hex  | 0xff    |
| oct  | 0o77    |
| bin  | 0b10_01 |
| byte | b'A'    |

# Numeric Types

| size    | signed | unsigned | fp      |
| :---    | :---   | :---     | :---    |
| 8-bit   | i8     | u8       |         |
| 16-bit  | i16    | u16      |         |
| 32-bit  | i32    | u32      | f32     |
| 64-bit  | i64    | u64      | f64     |
| 128-bit | i128   | u128     |         |
| arch    | isize  | usize    |         |

# Characters

`char` is a 4-byte (64-bit) UTF-8 character.

# Compounds

## Arrays
```rust
    let x: [i32; 5]  = [1,2,3,4,5];
    let y = [3;5]; // equivalent to: let y: [i32;5] = [3,3,3,3,3];

    let first_x = x[0];
    let second_y = y[1];
```

## Tuples

```rust
let tup: (i32, f64, u8) = (500, 5.0, 42);
let (x,y,z) = tup;
assert_eq!(x, tup.0);
assert_eq!(y, tup.1);
//...
```
