# Functions
`<name> :: <class constraint> => <function-type>`
- `even :: Integral a => a -> Bool`

## Conditional Expression
```haskell
if <condition> then <expression> else <expression>
```

## Guarded equations
```haskell
<name> <params> | <condition> = <expression>
                | <condition> = <expression>
```
```haskell
abs n | n >= 0 = n
      | otherwise = -n
```

## Pattern matching
```
not :: Bool -> Bool
not False = True
not True = False

(&&) :: Bool -> Bool -> Bool
True && b = b
False && _ = False
```

## Lambdas
```haskell
\<p1> -> <expression>
add = \x -> (\y -> x + y)
```