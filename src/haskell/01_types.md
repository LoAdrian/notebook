# Types
## Basic Types
- `Bool`: `True` or `False`
- `Char`: Unicode characters
- `String`: Equivalent to `Char[]`
- `Int`: $-2^63$ to $2^63 -1$
- `Integer`: Arbitrary-precision integer
- `Float`: Single-precision FP
- `Double`: Double-precision FP
- `[T]`: List of elements of the same type
- `(T1,T2)`: Tuple of elements of type `TN`
- `T1 -> T2`: Function mapping `T1` to `T2`
- `T1 -> (T2 -> T3)`: Curried function mapping `T1` to a function mapping `T2` to `T3`

## Basic Classes and their methods
- `Eq`: `(==)`, `(/=)`
- `Ord`: `(<)`, `(<=)`, `(>)`, `(>=)`, `min`, `max`
- `Show`: `show :: a -> String`
- `Read`: `read :: String -> a`
- `Num`: `(+)`, `(-)`, `(*)`, `(negate)`, `abs`, `signum`
- `Integral :: Num`: `div`, `mod`
- `Fractional :: Num`: `(/)`, `recip`