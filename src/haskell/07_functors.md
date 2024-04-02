# Functors
Functors are datatypes that implement:
```haskell
class Functor f where
   fmap :: (a -> b) -> f a -> f b
```

## map
```haskell
map :: (a -> b) -> [a] -> [b]
map f [] = []
map f (x:xs) = f x : map f xs
```
- Only implemented for lists

## fmap
```haskell
instance Functor [] where
	fmap = map

instance Functor Maybe where
	fmap _ Nothing = Nothing
	fmap g (Just x) = Just (g x)
	
data Tree a = Leaf a | Node (Tree a) (Tree a)
    deriving Show
instance Functor Tree where
   fmap g (Leaf x) = Leaf (g x)
   fmap g (Node l r) = Node (fmap g l) (fmap g r)
```
