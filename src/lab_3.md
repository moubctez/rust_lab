# Sterowanie przepływem

## Wyrażenia warunkowe

Wyrażenie `if` pozwala na wykonanie wskazanego bloku kodu w przypadku kiedy podany warunek jest
spełniony (jego wartość jest `true`).

```rust
let score = 50;
if score < 100 {
    println!("Warunek spełniony");
}
```

Opcjonalnie można wskazać blok kodu, który zostanie wykonany, kiedy warunek nie jest spełniony (jego
wartość jest `false`). Taki blok podajemy bezpośrednio po `if` używając słowa kluczowego `else` (co
oznacza _w przeciwnym wypadku_).

```rust
let score = 50;
if score < 100 {
    println!("Warunek spełniony");
} else {
    println!("Warunek niespełniony");
}
```

Warunek musi wyliczać się do typu `bool`. Inaczej kompilator zgłosi błąd.

```rust,compile_fail
let liczba = 10;
if liczba {
    println!("Liczba różna od zera");
}
```

```text
error[E0308]: mismatched types
 --> src/main.rs:3:8
  |
3 |     if liczba {
  |        ^^^^^^ expected `bool`, found integer
```

Prawidłowo program powinien wyglądać tak:

```rust
let liczba = 10;
if liczba != 0 {
    println!("Liczba różna od zera");
}
```

Wyrażenia `if` i `else` można łączyć w ciągi rozdzielone `else if`.

```rust
let score = 50;
if score < 10 {
    println!("Mało");
} else if score < 50 {
    println!("Więcej");
} else if score < 100 {
    println!("Dużo");
} else {
    println!("Za dużo");
}
```

Ponieważ `if` jest wyrażeniem, można go użyć do przypisania wartości w deklaracji `let`.

```rust
let score = 50;
let bonus = if score < 100 { 2 } else { 5 };
```

Należy pamiętać, że przy deklarowaniu zmiennej, wszystkie gałęzie `if` muszą zwracać wartość
jednakowego typu. W przeciwnym wypadku nastąpi błąd kompilacji.

```rust,compile_fail
let score = 50;
let bonus = if score < 100 { 2 } else { 5.0 };
```

Kompilacja powyższego kończy się błędem:

```text
error[E0308]: `if` and `else` have incompatible types
 --> src/main.rs:3:48
  |
3 |     let bonus = if score < 100 { 2 } else { 5.0 };
  |                                     -          ^^^ expected integer, found floating-point number
  |                                     |
  |                                     expected because of this
```

Pamiętaj również, że wszystkie możliwości muszą zostać obsłużone, czyli nie może być takiego
przypadku, że warunek nie zwróci wartości. Poniższy kod nie zadziała, ponieważ brakuje końcowego
`else` zwracającego wartość:

```rust,compile_fail
let score = 50;
let bonus = if score < 100 {
    2
} else if score > 0 {
    3
}
```

```text
error[E0317]: `if` may be missing an `else` clause
  --> src/main.rs:29:12
   |
29 |       } else if score > 0 {
   |  ____________^
30 | |         3
   | |         - found here
31 | |     };
   | |_____^ expected integer, found `()`
   |
   = note: `if` expressions without `else` evaluate to `()`
   = help: consider adding an `else` block that evaluates to the expected type
```

### Przykład: liczby dni w roku.

```rust
let year = 2025;
let days_in_year = if (year % 4 == 0 && year % 100 != 0) || year % 400 == 0 {
    366
} else {
    365
};
```
