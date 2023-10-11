# Structures



## Copy with

```lean
structure Pair where
  x : Nat
  y : Nat
deriving Repr

instance : ToString Pair where
  toString p := toString p.x ++ "," ++ toString p.y

def origin : Pair := {x := 0, y := 0}


def main: IO Unit := do

  println! origin
  
  let o1 :=  { origin with x:= 1 }
  println! o1

  let o2 :=  { origin with y:= 3 }
  println! o2
```
