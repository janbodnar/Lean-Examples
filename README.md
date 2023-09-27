# Lean-Examples
Lean 4 code examples

## Random value

To read from an IO, Lean uses the `<-` operator.  

```lean
def main: IO Unit := do

  let r <- IO.rand 0 99
  IO.println r
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

