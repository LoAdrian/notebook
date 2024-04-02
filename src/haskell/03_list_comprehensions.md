# List comprehensions
```haskell
<name> <param> = [<var> | <generators>, <guards>]

length :: [a] -> Int
length xs = sum [1 | _ <- xs]

factors :: Int -> [Int]
factors n = [x | x <- [1..n], n `mod` x == 0]

weirdList = [(x,y) | x <- [1..10], y <- [11..20]]
```