# `Vec<T>`
- Stores a list of elements

## Methods
- `new() -> Self`
- `push(&mut self, e: T)`
- `pop(&mut self) -> Option<T>`
- `clear(&mut self)`
- `len(&self) -> usize`
- `is_empty(&self) -> bool`

## Important Trait Implementations
- `Index<I>` (Implements Indexers)
- `IndexMut<I>` (Implements Mutable Indexers)


## Macros
- `vec![e1, e2, ...] -> Vec<E>`