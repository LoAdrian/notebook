# Applicatives
- Functors abstract mapping a function over each element of a structure.
- Applicatives are used to allow functions wwith any number of arguments to be mapped.

```haskell
fmap2 :: (a -> b -> c) -> f a -> f b -> f c
fmap2 (+) (Just 1) (Just 2)
-- >>> Just 3
```

```haskell
pure :: a -> f a
-- converts a value of type a into a structure of type f a
(<*>) :: f (a -> b) -> f a -> f b
-- Generalized form of function application for which the argument function, the argument value and the result value are all contained in f structures.
fmap2 :: (a -> b -> c) -> f a -> f b -> f c
fmap2 g x y = pure g <*> x <*> y
```

## Definition
```haskell
class Functor f => Applicative f where
   pure :: a -> f a
   (<*>) :: f (a -> b) -> f a -> f b
```
- The spaceship operator is left-associative

## Examples
```haskell
instance Applicative Maybe where
   pure = Just
   Nothing <*> _ = Nothing
   (Just g) <*> mx = fmap g mx
pure (+1) <*> Just 1
-->> Just 2
pure (+) <*> Just 1 <*> Just 2
-->> Just 3
pure (+) <*> Nothing <*> Just 2
-->>Nothing
```

```haskell
instance Applicative [] where
   pure x = [x]
   gs <*> xs = [g x | g <- gs, x <- xs]
```