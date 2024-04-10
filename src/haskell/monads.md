# Monads
```haskell
(>>=) :: Maybe a -> (a -> Maybe b) -> Maybe b
mx >>= f = case mx of 
   Nothing -> Nothing
   Just x -> f x
```
- `>>=` takes an argument of type a that may fail and a function of type `a -> b` whose result may fail, and returns a result of type b that may fail.

## Definition
```haskell
class Applicative m => Monad m where
   return :: a -> m a
   (>>=) :: m a -> (a -> m b) -> m b
   return = pure
```
- default return is pure (applicative) but can bve overriden

## Syntax

```haskell
m1 >>= \x1 -> (m2 >>= \x2 -> mn >>= \xn -> f x1 x2 xn)
-- is equivalent to
do x1 <- m1
   x2 <- m2
   xn <- mn
   f x1 x2 xn
```
