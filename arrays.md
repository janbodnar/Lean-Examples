# Arrays



## Definition 

Arrays are created with `mkArray` function or with the `#[]` notation.  

```lean
def main : IO Unit := do

  let as := mkArray 5 'a'
  IO.println as

  let zeros := mkArray 5 0
  IO.println zeros

  let ones := zeros.map (λ e => e + 1)
  IO.println ones

  IO.println "--------------------"

  let vals := #[1, 2, 3, 4, 5]
  IO.println vals
  IO.println vals[0]
  IO.println vals[1]
  IO.println vals[11]? -- print none if idx does not exist

  for e in vals do 
    IO.println e
```

## List to array

```lean
def vals := Array.mk [1, 2, 3, 4, 5]
#eval vals
```

## forM

```lean
def nums := Array.mk [1, 2, 3, 4, 5]
#eval nums.forM IO.println
```

## Modification

```lean
def main : IO Unit := do

  let a := Array.mk [1, 2, 3, 4, 5]

  -- makes a copy of a (since a is still referenced)
  -- and modifies element in position 3
  let b := a.set! 3 44
  let b := b.set! 4 55  -- modifies b in-place

  IO.println a
  IO.println b
```
