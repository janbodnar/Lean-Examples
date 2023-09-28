# Strings


## String length

```lean
def main: IO Unit := do

  let word := "falcon"
  IO.println (String.length word)

  let word2 := "вселенная"
  IO.println (String.length word2)

  let word3 := "नमस्ते"
  IO.println (String.length word3)
```
