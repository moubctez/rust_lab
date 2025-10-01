# Operatory

## Logiczne i arytmetyczne

| symbol | `bool`        | liczby całkowite     | liczby zmiennoprzecinkowe |
|--------|---------------|----------------------|---------------------------|
| `+`    |               | dodawanie            | dodawanie                 |
| `-`    |               | odejmowanie          | odejmowanie               |
| `*`    |               | mnożenie             | mnożenie                  |
| `/`    |               | dzielenie            | dzielenie                 |
| `&`    | logiczne I    | bitowe I             |                           |
| `\|`   | logiczne LUB  | bitowe LUB           |                           |
| `^`    | logiczne ALBO | bitowe ALBO          |                           |
| `<<`   |               | przesunięcie w lewo  |                           |
| `>>`   |               | przesunięcie w prawo |                           |

## Porównawcze

| symbol | znaczenie          |
|--------|--------------------|
| `==`   | równy              |
| `!=`   | różny              |
| `>`    | większy            |
| `<`    | mniejszy           |
| `>=`   | większy lub równy  |
| `<=`   | mniejszy lub równy |

## Leniwe booleanowskie

| symbol | znaczenie    |
|--------|--------------|
| `&&`   | logiczne I   |
| `\|\|` | logiczne LUB |

W przykładzie poniżej, z powodu lenistwa, `panic!()` nie będzie wykonywany.

```rust
let a = true || panic!();
let b = false && panic!();
```
