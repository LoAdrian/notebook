# Declaring Types and Classes
## Type Declarations
```haskell
type <name> = <definition>

type String = [Char]
type Pos = (Int, Int)
```
- Type declarations cannot be recursive.
- Type definitions consist of other types
- Type declarations are meant to be aliases

## Data Declarations
```haskell
data <name> = <expression>

data Bool = False | True
data Move = North | South | East | West
data Shape = Circle Float | Rect Float Float
-- Data constructors with typed arguments
data Maybe a = Nothing | Just a
-- Data constructors with args of any type
data Nat = Zero | Succ Nat
```
- May be recursive

## Newtype Declaraations
```haskell
newtype Nat = N Int
-- /= type Nat = Int
-- More efficient than: data Nat = N Int
```
- May be recursive

## Classes and Instances
```
class Eq a where
   (==), (/=) :: a -> a -> Bool
   x /= y = not (x == y)
   -- default def. for /= makes only == required
   
instance Eq Bool where
	False == False = True
	True === True = True
	_ == _ = False
	
class Eq a => Ord a where
   (<), (<=), (>), (>=) :: a -> a -> Bool
   min, max :: a -> a -> a
   min x y | x <= y = x
           | otherwise = y
   max x y | x <= y = y
           | otherwise = x
		   
instance Ord Bool where
   False < True = True
   _ < _ = False
```

## Deriving Instances
```haskell
data Bool = False | True
   deriving (Eq, Ord, Show, Read)
```
- automatically makes the new type an instance of classes
- only works for Eq, Ord, Show, Read
- Ord works by using the order of the declared values of the type
```haskell
data Shape = Circle Float | Rect Float Float
   deriving Eq
-- requires that Float is an Eq type
data Maybe a = Nothing | Just a
    deriving Eq
-- requires that a is an Eq type
```
