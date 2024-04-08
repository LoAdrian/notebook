# Interactive Programming
> In Haskell, an interactive program is a pure function that takes the current state of the world as its argument and produces a modified world as its results

## Basic Actions
```haskell
getChar :: IO Char
getChar = ...
putChar :: Char -> IO ()
putChar c = ...
return :: a -> IO a
return v = ...
```

## Sequencing
```haskell
act :: IO (Char, Char)
act = do x <- getChar
         getChar
         y <- getChar
         return (x, y)
```
