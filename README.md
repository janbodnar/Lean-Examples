# Lean-Examples
Lean 4 code examples

## Random value

To read from an IO, Lean uses the `<-` operator.  

```lean
def main: IO Unit := do

  let r <- IO.rand 0 99
  IO.println r
```

## for loop

```lean
def main : IO Unit := do 

  let words := ["sky", "rock", "ten", "water", "cloud"]

  for word in words do
    IO.println word

  let vals := List.range 10

  for e in vals do
    IO.println e
```

## Function definition

A function is defined with the `def` keyword.  There is no `return` keyword.  

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

## Pipes

```lean
def add1 x := x + 1
def times2 x := x * 2

def main : IO Unit := do

  IO.println (times2 (add1 55))
  IO.println (55 |> add1 |> times2)
  IO.println (times2 <| add1 <| 55)
```

## Structure

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

## map/filter

```lean
def fn (xs : List Nat) :=
  List.filter (· % 2 == 0) (List.map (· * 3) (List.map (· + 1) xs))

def main: IO Unit := do

  let res := fn [1, 2, 3, 4]
  IO.println res

  let res := fn <| [1, 2, 3, 4]
  IO.println res

  let res := [1, 2, 3, 4] |> fn 
  IO.println res

  -- Haskell version
  let res2 := fn $ [2, 5, 6, 9, 10]
  IO.println res2
```

## Lambdas 

`(·*1)` is syntax sugar for `fun e => e*1`.  
The `λ` is written with `\lambda`.  

```lean
def main : IO Unit := do

  let vals := [1, 2, 3, 4, 5, 6]

  let res := List.map (fun e => e * 2) vals
  IO.println res

  let res := List.map (λ e => e * 3) vals
  IO.println res

  let res := List.map (.* 4) vals
  IO.println res
```

```lean


def double (n : Nat) : Nat := n + n

def mand (a b : Bool) : Bool :=
  match a with
  | true  => b
  | false => false


def main : IO Unit := do

  IO.println (mand true true)
  IO.println (mand true false)
  IO.println (mand false true)
  IO.println (mand false false)

  IO.println (double 5)

  let res := List.map (λ x => toString x) [1, 2, 3]
  IO.println res

  let res2 := [1, 2, 3, 4, 5, 6, 7].map (λ x => x + 1)
  IO.println res2

  for e in List.range 10 do
    IO.println e

  let data := (List.range 10).map (λ x => x * x)
  IO.println data
```





## Match pattern 

```lean
inductive Weekday where
  | sunday | monday | tuesday | wednesday
  | thursday | friday | saturday

open Weekday

def natOfWeekday (d : Weekday) : Nat :=
  match d with
  | sunday => 1
  | monday => 2
  | tuesday => 3
  | wednesday => 4
  | thursday => 5
  | friday => 6
  | saturday => 7


def main : IO Unit := do

  IO.println (natOfWeekday Weekday.friday)
  IO.println (natOfWeekday Weekday.monday)
  IO.println (natOfWeekday Weekday.sunday)
```

```lean
open Nat

-- def swap : α × β → β × α
--   | (a, b) => (b, a)

def isZero : Nat → Bool
  | zero   => true
  | succ x => false

def fib : Nat -> Nat
  | 0 => 0
  | 1 => 1
  | n+2 => fib (n+1) + fib n

def main : IO Unit := do

  IO.println (fib 10)
  IO.println (fib 20)

  IO.println (isZero 10)
  IO.println (isZero 0)
  IO.println (isZero 4)
```

## Fibonacci 

```lean
-- Naive version
def fib1 (n : Nat) : Nat :=
  match n with
  | 0 => 0
  | 1 => 1
  | (k + 2) => fib1 k + fib1 (k + 1)

-- More efficient version
def fib_aux (n : Nat) (a : Nat) (b : Nat) : Nat :=
  match n with
  | 0 => b
  | (k + 1) => fib_aux k (a + b) a
 
def fib2 (n : Nat) : Nat :=
  fib_aux n 1 0

def main : IO Unit := do
  IO.println (fib2 5)
  IO.println (fib1 15)
```

## Fizz/buzz

```lean
def fizz : String :=
  "Fizz"

def buzz : String :=
  "Buzz"

def newLine : String :=
  "\n"

def isDivisibleBy (n : Nat) (m : Nat) : Bool :=
  match m with
  | 0 => false
  | (k + 1) => (n % (k + 1)) = 0

def getTerm (n : Nat) : String :=
  if (isDivisibleBy n 15) then (fizz ++ buzz)
  else if (isDivisibleBy n 3) then fizz
  else if (isDivisibleBy n 5) then buzz
  else toString (n)

def range (a : Nat) (b : Nat) : List (Nat) :=
  match b with
  | 0 => []
  | m + 1 => a :: (range (a + 1) m)

def getTerms (n : Nat) : List (String) :=
  (range 1 n).map (getTerm) 

def addNewLine (accum : String) (elem : String) : String :=
  accum ++ elem ++ newLine

def fizzBuzz : String :=
  (getTerms 100).foldl (addNewLine) ("")

def main : IO Unit :=
  IO.println (fizzBuzz)
```

