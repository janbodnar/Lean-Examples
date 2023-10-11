# Structures

Structure is a special case of inductive datatype. It has only one constructor  
and is not recursive.

The typeclass instance is where the operations described in the typeclass declaration  
are implemented for a particular type. An instance is a unique relationship  
between a type and a typeclass.

## Constructor

The `mk` is the default constructor.  

```lean
structure User where
  name : String  
  occupation : String

instance: ToString User where
  toString p := p.name ++ ": " ++ p.occupation

def u := User.mk "John Doe" "gardener"


def main: IO Unit := println! u
```

## Custom constructor 

```lean
structure User where
  name : String  
  occupation : String

instance: ToString User where
  toString p := p.name ++ ": " ++ p.occupation

-- def User.make (_name : String) (_occupation: String): User :=
--   { name := _name, occupation := _occupation}

def User.make (_name _occupation: String): User :=
  { name := _name, occupation := _occupation}

def u1 := User.mk "John Doe" "gardener"
def u2 := User.make "Roger Roe" "driver"

def main: IO Unit := do 
  println! u1
  println! u2
```



## Definition

```lean
structure User where
  name : String
  age  : Nat

instance : ToString User where
  toString p := p.name ++ "@" ++ toString p.age


def main : IO Unit := do

  let p := { name := "Leo", age := 44 : User }

  IO.println p.name
  IO.println p.age
  IO.println p

  let p: User := { name := "Peter", age := 23 }
  IO.println p

  let p: User := { 
    name := "Lucia"
    age := 29
  }
  
  IO.println p

  p |> IO.println
```

## Functions

Various syntax for functions & structures.  

```lean
structure Point where
  x : Nat
  y : Nat

instance : ToString Point where
  toString p := toString p.x ++ "," ++ toString p.y

def add (p q : Point) : Point :=
  { x := p.x + q.x, y := p.y + q.y }

def Point.add (p q : Point) : Point :=
  { x := p.x + q.x, y := p.y + q.y }

def Point.add2 (p q : Point) : Point := {
   x := p.x + q.x
   y := p.y + q.y 
}

def Point.add3 (p q : Point) : Point where
   x := p.x + q.x
   y := p.y + q.y 


def main: IO Unit := do 

  println! Point.add (Point.mk 1 2) (Point.mk 3 3)
  println! Point.add2 (Point.mk 1 2) (Point.mk 3 3)
  println! Point.add3 (Point.mk 1 2) (Point.mk 3 3)

  println! add (Point.mk 1 2) (Point.mk 1 2)
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

The expression `p.add q` is a shorthand for `Point.add p q`  

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
