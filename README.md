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

