# Creates
- Smallest considerable amount of code
- Binary crates are executable
- Library crates are not

# Packages
- Bundle of one or more crates
- Contains a `cargo.toml` file that describes how to build the contained crates
- Contains at most one library crate and any amount of binary crates

# Modules 
- Control scope and privacy
## Modern declaration
```
myProject
├── Cargo.lock
├── Cargo.toml
└── src
    ├──garden
       ├──vegetables.rs
    ├──garden.rs //mod vegetables.rs
    ├──main.rs //pub mod garden;
```
## Old declaration
```
myProject
├── Cargo.lock
├── Cargo.toml
└── src
    ├──garden
       ├──mod.rs
       ├──vegetables
          ├──mod.rs
    ├──main.rs //pub mod garden;
```
## Inline declaration

```rust
mod vegetables {
  //...
}
```
# Module Paths
```rust
mod X  {
  pub fn x() {
    Y::y(); //relative path
  }

  pub mod Y {
    struct YS;
    pub fn y() {
      super::x(); //relative super-path
    }
  }
}

fn main() {
  crate::X::Y::y(); //absolute path
}
```

# Renaming usings with `as`
```rust
use X::Y::YS as XS;
```

# Nested Paths
```rust
use X::Y::{YS, y};
```

# Glob operator
```rust
use X::{*, Y::*};
```
- brings all `pub` items into scope

# Visibility modifiers
```rust
//None: private
pub(self) XY; //private
pub XY; //public
pub(crate) XY; //public to this crate
pub(super) XY; //public to the parent module
pub(in PATH) //public to the module in PATH where PATH is the path to an ancestor module
```

# `pub use` reexporting
- `pub use MODULE` reexports the module under the current module namespace
