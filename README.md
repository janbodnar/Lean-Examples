# Lean-Examples
Lean 4 code examples

`lean --run Hello.lean` - run a Lean program

## New project

- `lake init hello`
- `lake new hello`

## Simple program 

The `println!` is a macro.  

```lean
def main: IO Unit :=

  println! "Hello there!"
```

## Lean version/origin

The `Lean.versionString` and `Lean.origin` definitions store the version and  
origin of Lean.  

```lean
def main: IO Unit := do

  println! Lean.versionString
  println! Lean.origin
```

## main arguments

Run the program with `lean -r args.lean 1 2 3`  

```lean
def main(_args: List String): IO UInt32 := do

    _args.forM λ e => println! e

    println! "----------------"

    _args.forM IO.println

    println! "----------------"

    _args.forM (println! .)

    println! "----------------"

    _args |>.forM IO.println

    println! "----------------"

    for e in _args do
      println! e

    return 0
```


## Nat lower bound is 0

The `1 - 3` evaluates to `0` for natural numbers.  

```lean
#eval 1 - 3
#eval 1 - (3 : Int)
#eval (1 - 3 : Int)
```

## Mutable variables 

Mutable variables are created with `mut`.  

```lean
def main: IO Unit := do

  let mut x := 10

  for _ in [:5] do 
    x := x + 1

  IO.println x
```

## Not equal 

Lean uses the classic `!=` operator or the `≠`.  
The `≠` is inserted with `\neq` or `\ne` or `\eqn` or `\=n`.  

```lean
def main: IO Unit := do

  let n := 1
  let m := 2

  if m != n then
    IO.println "m is not equal to n"

  if m ≠ n then
    IO.println "m is not equal to n"
```

## if is an expression

The `if` form can return values.  

```lean
def main: IO Unit := do

  let n := 1
  let m := 1

  if m ≠ n then IO.println "m is not equal to n" else IO.println "m is equal to n"

  let msg := if m ≠ n then "m is not equal to n" else "m is equal to n"
  IO.println msg
```

## The by keyword

The `by` keyword runs a tactic. It can be used both in theorems and normal programs.  
We create a safe type for the divide operation by ensuring that the divisor is not equal to zero.  

```lean
notation "ℕ" => Nat

example : 2 + 13 ≠ 5 := by decide

def divide (x : ℕ) (y : { n : ℕ // n ≠ 0 }) : ℕ := x / y
#check divide 4 ⟨3, by decide⟩

def main : IO Unit := do

  let r := divide 4 ⟨2, by decide⟩
  println! r
```



## String concatenation 

We can use the `++` operator or the `String.append` function.  

```lean
def main: IO Unit :=

  let s1 := "an old"
  let s2 := "falcon"

  let msg := s1 ++ " " ++ s2
  IO.println msg
```

```lean
def main: IO Unit :=

  let s1 := "an old"
  let s2 := "falcon"

  let msg := String.append (String.append s1 " ") s2
  IO.println msg
```

## String interpolation

String interpolation is performed with `s!""`. The definitions are expanded inside `{}`.  

```lean
def main: IO Unit :=
  let name := "John Doe"
  let age := 34

  let msg := s!"{name} is {age} years old"
  IO.println msg
```

## Conditions 

Lean uses `if/else/else if/then` to create conditions.  

```lean
def main: IO Unit := do 

  let vals := [-3, -2, 0, -1, 2, 1, 3]
  
  for e in vals do
    if e < 0 then
      IO.println s!"{e} is negative"
    else if e > 0 then
      IO.println s!"{e} is positive"
    else
      IO.println s!"{e} zero"
```

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

## Ranges 

```lean
def main: IO Unit := 

  for e in [1:20:3] do 
    IO.println e
```



## Notation 

We can use Greek letters with `notation`.  

```lean
notation "ℕ" => Nat

def maximum (n : ℕ) (k : ℕ) : ℕ :=

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

## Defining new type functions

In Lean, we can define new functions both for the type and instance.  

```lean
-- def List.sum' [Add α] [Inhabited α] (l : List α) := l.foldl (· + ·) default
def List.sum' [Add α] [Inhabited α] (l : List α) := l.foldl (λ x y => x + y) default

def main: IO Unit := do

  let vals: List Nat := [1, 2, 3, 4, 5]
  IO.println vals.sum'

  let vals: List Nat := [1, 2, 3, 4, 5]
  IO.println (List.sum' $ vals)

  let vals: List Int := [-2, -1, 0, 1, 2, 3, 4, 5]
  IO.println vals.sum'

  let vals: List Float := [1.1, 2.2, 3.3, 4.4, 5.5]
  IO.println vals.sum'
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

## Twice function 

```lean
-- def twice (f : Nat → Nat) (a : Nat) :=
--   f (f a)

def twice (f : α -> α) := f ∘ f


def main: IO Unit := do

  IO.println (twice (.+ 5) 10)
  IO.println (twice (.* 5) 10)
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

