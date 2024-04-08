# Higher Order Functions
## `foldr`
- Associates to the right
- Used to implement operators associating to the right
```haskell
foldr :: (a -> b -> b) -> b -> [a] -> b
foldr f v [] = v
foldr f v (x:xs) = f x (foldr f v xs)
-- takes a binary operator and start value
-- returns a function taking [a], returning b

sum :: Num a => [a] -> a
sum = foldr (+) 0

product :: Num a => [a] -> a
product = foldr (*) 1

or :: [Bool] -> Bool
or = foldr (||) False

and :: [Bool] -> Bool
and = foldr (&&) True
```

## `foldl`
```haskell
foldl :: (a -> b -> a) -> a -> [b] -> a
foldl f v [] = v
foldl f v (x:xs) foldl f (f v x) xs
```

## Composition operator `(.)`
```haskell
(.) :: (b-> c) -> (a -> b) -> (a -> c)
f . g = \x f (g x)
```