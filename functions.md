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
