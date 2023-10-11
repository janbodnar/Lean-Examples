# Structures


## Definition

```lean
structure Person where
  name : String
  age  : Nat
deriving Repr

instance : ToString Person where
  toString p := p.name ++ "@" ++ toString p.age


def main : IO Unit := do

  let p := { name := "Leo", age := 44 : Person }

  IO.println p.name
  IO.println p.age

  IO.println p
```

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


## Generic structure

```lean
structure Point (α : Type u) where
  x : α
  y : α
-- deriving Repr

instance: ToString (Point Nat) where
  toString p := toString p.x ++ "," ++ toString p.y

def Point.add (p q : Point Nat) : Point Nat :=
  { x := p.x + q.x, y := p.y + q.y }

def p : Point Nat := Point.mk 1 2
def q : Point Nat := Point.mk 3 4


def main: IO Unit := do

  println! p.x
  println! p.y
  println! (toString p)
  println! p

  let p1 := { x:= 1, y := 1: Point Nat }
  let p2 := { x:= 2, y := 2: Point Nat }

  let p3 := Point.add p1 p2
  println! p3

  println! p1.add p2
```
