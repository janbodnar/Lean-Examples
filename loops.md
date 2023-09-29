# Loops 


## For each loop

```lean
def main : IO Unit := do 

  let words := ["sky", "rock", "ten", "water", "cloud"]

  for word in words do
    IO.println word

  let vals := List.range 10

  for e in vals do
    IO.println e
```


## Parallel loops

```lean
def main : IO Unit := do 

  let words := ["sky", "rock", "ten", "water", "cloud"]

  let n := words.length
  for x in words, y in [1:n] do
    IO.println (x, y)
```

## While loop

```lean
def main: IO Unit := do

  let vals := [1, 2, 3, 4, 5]

  let n := vals.length
  let mut i := 0
  let mut sum := 0

  while i < n do 

    sum := sum + vals[i]!
    i := i + 1

  IO.println sum
```
