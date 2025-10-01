# Podstawowe typy danych

Rust definiuje następujące podstawowe typy danych:

* Liczby całkowite
  - ze znakiem: `i8`, `i16`, `i32`, `i64`, `i128`, `isize`
  - bez znaku: `u8`, `u16`, `u32`, `u64`, `u128`, `usize`
* Liczby zmiennoprzecinkowe: `f32`, `f64`
* Booleanowskie: `bool`
* Znak: `char`
* Krotka: _tuple_ `(x, y, …)`
* Tablica: _array_ `[a, b, …]`
* Wskaźnik: `pointer`
* Referencja: `reference`
* Wycinek: _slice_ `&[a, b, …]`
* Ciąg znaków: `str`
* Jedność: _unit_ `()`

## Liczby całkowite

Domyślnym typem jest `i32`.

### Zakres

#### Liczby całkowite ze znakiem

| typ  | bajtów | wartość minimalna                        | wartość maksymalna                      |
|------|-------:|-----------------------------------------:|----------------------------------------:|
| i8   |      1 |                                     -128 |                                     127 |
| i16  |      2 |                                   -32768 |                                   32767 |
| i32  |      4 |                              -2147483648 |                              2147483647 |
| i64  |      8 |                     -9223372036854775808 |                     9223372036854775807 |
| i128 |     16 | -170141183460469231731687303715884105728 | 170141183460469231731687303715884105727 |

#### Liczby całkowite bez znaku

| typ  | bajtów | wartość minimalna | wartość maksymalna                      |
|------|-------:|------------------:|----------------------------------------:|
| u8   |      1 |                 0 |                                     255 |
| u16  |      2 |                 0 |                                   65536 |
| u32  |      4 |                 0 |                              4294967295 |
| u64  |      8 |                 0 |                    18446744073709551615 |
| u128 |     16 |                 0 | 340282366920938463463374607431768211455 |

### Literały

Podkreślenia (`_`) są opcjonalne.

* liczby dziesiętne: `123_456`
* liczby szesnastkowe: `0xdead_f00d`
* liczby ósemkowe: `0o644`
* liczby binarne: `0b1100_1010`
* znak jako bajt (tylko `u8`): `b'X'`

## Liczby zmiennoprzecinkowe

Liczby zmiennoprzecinkowe są reprezentowanie zgodnie ze standardem IEEE-754. Domyślnym typem jest
`f64`.

```rust
let x = 2.0; // f64
let y: f32 = 3.0; // f32
```

## Boolean

Typ `bool` przyjmuje tylko dwie wartości:

* prawda `true`
* fałsz `false`

```rust
let can_continue: bool = true;
let toxic = false;
```

## Znaki

Typ `char` reprezentuje pojedynczy znak zgodnie ze standardem [Unicode](https://home.unicode.org/).

```rust
let a: char = 'a';
let smiling_face = '😀';
```

## Krotki

Typ `tuple` grupuje różne typy danych z jeden złożony typ.

```rust
let set: (char, i32, &str) = ('🍎', 100, "Jabłko");
```

Można „dobrać” się do poszczególnych elementów krotki:

```rust
let set = ('🍎', 100, "Jabłko");
println!("Przedmiot {}", set.0);
println!("Punkty {}", set.1);
println!("Nazwa {}", set.2);
```

Można też „zniszczyć” krotkę poprzez wyciągnięcie z niej elementów.

```rust
let set = ('🍎', 100, "Jabłko");
let (object, value, name) = set;
println!("Przedmiot {object}");
println!("Wartość {value}");
println!("Nazwa {name}");
```

## Tablica

Tablica jest zbiorem elementów tego samego typu. Raz zdefiniowana tablica nie może zmieniać liczby
zawartej w niej elementów. (Do tego służą inne type, np. `Vec`).

```rust
let fib: [u16; 9] = [1, 2, 3, 5, 8, 13, 21, 34, 55];
let weekdays = ["poniedziałek", "wtorek", "środa", "czwartek", "piątek", "sobota",
                "niedziela"];
```

Można zdefiniować tablicę zawierającą określoną liczbę takich samych elementów:

```rust
let all_fives = [5; 3]; // to samo co [5, 5, 5]
```

Do poszczególnym elementów tablicy można „dobrać” poprzed wskazanie indeksu w nawiasie kwadratowym.
Indeksy zaczynają się od 0.

```rust
let fib = [1, 2, 3, 5, 8, 13, 21, 34, 55];
let eight = fib[4];
```

Próba pobrania elementu z indeksem spoza zakresu kończy się błędem.

```rust,compile_fail
let fib = [1, 2, 3, 5, 8, 13, 21, 34, 55];
let non_existing = fib[9];
```

## Ciąg znaków

```rust
let text = "Zażółć gęślą jaźń.";
```
