# Functions


## Function definition

A function is defined with the `def` keyword.  

```lean
def maximum (n : Nat) (k : Nat) : Nat :=
  if n < k then
    k
  else n

def main: IO Unit := do

  IO.println (maximum 10 5)
  IO.println (maximum 10 11)
  IO.println (maximum 12 11)
  IO.println (maximum (12 - 1) (7 + 4))
```

## Function calls 

Functions are called with `()` notation.  

```lean
def main: IO Unit := do 

  IO.println Nat.zero
  IO.println (Nat.succ Nat.zero)
  IO.println (Nat.succ (Nat.succ Nat.zero))
```


## Parameters 

The parameters can be defined in two ways: 

```lean
def add (x y : Nat) :=
  x + y

def add' (x : Nat) (y : Nat) :=
  x + y

def main: IO Unit := do 

  IO.println (add 2 3) 
  IO.println (add' 12 13)
```


## Function expression 

```lean
def double : Nat → Nat := fun
  | 0 => 0
  | k + 1 => double k + 2

def double2 : Nat → Nat :=
  fun x => x + x

def main: IO Unit := do

  let r := double 10
  IO.println r

  IO.println (double 2 + 1)
  IO.println (double2 12)
```


## Generic functions

```lean
variable (α β γ : Type)

def doTwice (h : α → α) (x : α) : α :=
  h (h x)

def doThrice (h : α → α) (x : α) : α :=
  h (h (h x))

def main: IO Unit := do

  let n := 4

  -- specify type following function name
  let res := doTwice Nat (fun e => e + 1) n
  IO.println res

  let res := doThrice Nat (.+ 5) 10
  IO.println res
```