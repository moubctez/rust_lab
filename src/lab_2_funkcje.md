# Funkcje

Funkcje to zestaw instrukcji manipulujący danymi. Jest to dominujący twór w kodzie programu. W Rust
funkcje definiowane są przy użyciu słowa kluczowego `fn`. Konwencjonalnie nazwy funkcji pisane są
małymi literami oraz podkreśleniami.

Główną funkcją, od której startuje program, jest funkcja `main()`.

```rust
fn main() {
    println!("Witaj, świecie!");

    przykładowa_funkcja();
}

fn przykładowa_funkcja() {
    println!("Pozdrowienia z przykładowej funkcji.");
}
```

W powyższym przykładzie, funkcja `main()` wywołuje inną funkcję `przykładowa_funkcja()`.

> `println!` to makro, czyli coś co zostanie rozwinięte podaczas kompilowania programu. Makro
> poznajemy po wykrzykniku `!` na końcu nazwy.

## Argumenty

Funkcje mogą przyjmować argumenty (zwane również parametrami funkcji). Argumenty są specjalnymi
zmiennmi tworzącymi _sygnaturę_ funkcji. Argumenty muszą mieć wskazany typ danych.

```rust
fn powitanie(imię: &str) {
    println!("Witaj, {imię}");
}

fn main() {
    powitanie("księżniczko");
    powitanie("książę");
}
```

Wywołanie funcji z nieodpowiednim typem danych jako argument kończy się błędem kompilowania.

```rust,compile_fail
fn powitanie(imię: &str) {
    println!("Witaj, {imię}");
}

fn main() {
    powitanie(0xc3);
}
```

```text
error[E0308]: mismatched types
 --> src/main.rs:6:15
  |
6 |     powitanie(0xc3);
  |     --------- ^^^^ expected `&str`, found integer
  |     |
  |     arguments to this function are incorrect
```

## Deklaracje i wyrażenia

Deklaracje to instrukcje, które wykonują pewne czynności i nie zwracają wartości. Deklaracje kończą
się średnikiem.

```rust
let wiek = 33;
```

Deklaracje nie zwracają wartości, więc nie można przypisać ich wyniku do zmiennej.

```rust
let wynik = (let wiek = 33);
```

```text
error: expected expression, found `let` statement
 --> src/main.rs:2:18
  |
2 |     let wynik = (let wiek = 33);
  |                  ^^^
  |
  = note: only supported directly in conditions of `if` and `while` expressions
```

Wyrażenia są przeliczanie i zwracają wartość.

```rust
let a = 1;     // '1' jest wyrażeniem
let b = 2 + 3; // '2 + 3' jest wyrażeniem
let c = {
    let d = 4; // '4' jest wyrażniem
    d + 5      // 'd + 5' jest wyrażeniem
};             // cały blok między nawiasami klamrowymi jest wyrażeniem
```

## Funkcje zwracające wartość

Funkcje mogą zwracać wartość. Zwracane wartości nie powiadają nazwy, ale muszę mieć okreśony typ
danych. Zwracane wartości definiowane są przez użycie strzałki `->` (dwa znaki). Zwracana wartość
jest ostatnim wyrażeniem w definicji funkcji (bez średnika).

```rust
fn suma(a: i32, b: i32) -> i32 {
    a + b
}

fn main() {
    let c = suma(16, 128);
    println!("Suma: {c}");
}
```

Może również zwracać wartość używając słowa kluczowego `return`. Służy to głównie do zwracania
wartości wcześniej, jeśli z jakiegoś powodu zachodzi taka potrzeba. Prefereowana jest składnia
przedstawiona powyżej (bez `return`).

```rust
fn suma(a: i32, b: i32) -> i32 {
    return a + b;
}
```
