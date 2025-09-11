# Sterowanie przepływem

## Wyrażenia warunkowe

Wyrażenie `if` pozwala na wykonanie wskazanego bloku kodu w przypadku kiedy podany warunek jest
spełniony (jego wartość jest `true`).

```rust
let punkty = 50;
if punkty < 100 {
    println!("Warunek spełniony");
}
```

Opcjonalnie można wskazać blok kodu, który zostanie wykonany, kiedy warunek nie jest spełniony (jego
wartość jest `false`). Taki blok podajemy bezpośrednio po `if` używając słowa kluczowego `else` (co
oznacza _w przeciwnym wypadku_).

```rust
let punkty = 50;
if punkty < 100 {
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
let punkty = 50;
if punkty < 10 {
    println!("Mało");
} else if punkty < 50 {
    println!("Więcej");
} else if punkty < 100 {
    println!("Dużo");
} else {
    println!("Za dużo");
}
```

Ponieważ `if` jest wyrażeniem, można go użyć do przypisania wartości w deklaracji `let`.

```rust
let punkty = 50;
let dodatek = if punkty < 100 { 2 } else { 5 };
```

Należy pamiętać, że przy deklarowaniu zmiennej, wszystkie gałęzie `if` muszą zwracać wartość
jednakowego typu. W przeciwnym wypadku nastąpi błąd kompilacji.

```rust,compile_fail
let punkty = 50;
let dodatek = if punkty < 100 { 2 } else { 5.0 };
```

Kompilacja powyższego kończy się błędem:

```text
error[E0308]: `if` and `else` have incompatible types
 --> src/main.rs:3:48
  |
3 |     let dodatek = if punkty < 100 { 2 } else { 5.0 };
  |                                     -          ^^^ expected integer, found floating-point number
  |                                     |
  |                                     expected because of this
```

Przykład: liczby dni w roku.

```rust
let rok = 2025;
let dni_w_roku = if (rok % 4 == 0 && rok % 100 != 0) || rok % 400 == 0 {
    366
} else {
    365
};
```
