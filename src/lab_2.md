# Laboratorium 2: Podstawowe typy i zmienne, podstawy

## Komentarze

Komentarze w kodzie zaczynają się od dwóch ukośników `//` i kończą się do prawej strony linijki.

```rust
// Główna funkcja programu.
fn main() {
    // Wypisanie komunikatu.
    println!("Hello, world!");
}
```

Komentarze mogą też zawierać się pomiędzy `/*` i `*/`. Ta forma komentowania jest mniej popularna.

```rust
/*
 * Główna funkcja programu.
 * Od tego zaczyna się program.
 */
fn main() {
    /*
     * Wypisanie komunikatu.
     * `println!` jest makro definicją.
     */
    println!("Hello, world!");
}
```

## Definiowanie zmiennych

Rust jest językiem statycznie typowanym, co oznacza, że podczas kompilacji musi znać rodzaj danych
przypisanych do zmiennych.

Zmienne definiowane są przez słowo kluczowe `let`, po którym podajemy nazwę zmieniej.
Konwencjonalnie nazwy funkcji pisane są małymi literami oraz podkreśleniami.

```rust
let c: i32 = 299_792_458;
println!("Prędkość światła w próżni {c} m/s.");
```

W powyższym przykładzie definicja powoduje, że nazwa `c` będzie miała przypisaną wartość
299792458 o typie `i32`. Definiując zmienne, typ danych można pominąć – kompilator powinien domyślić
się jakiego typu jest to zmienna. W przypadku liczb całkowitych, domyślnie jest to `i32`. Tak więc
poniższy kod jest równoważny poprzedniemu.

```rust
let c = 299_792_458;
println!("Prędkość światła w próżni {c} m/s.");
```

> Dobrą praktyką jest nie podawanie typów danych i pozwalanie kompilatorowi na odgadnięcie. Nie
> zawsze będzie to możliwe. Dobry edytor (IDE) pozwala na podejrzenie jakiego typu jest zmienna.

Nazwy zmiennnych nie mogą być słowami kluczowymi języka Rust.

```rust,compile_fail
let fn = 0
```

Kompilacja skończy się błędem:

```text
error: expected identifier, found keyword `fn`
 --> src/main.rs:2:9
  |
2 |     let fn = 0;
  |         ^^ expected identifier, found keyword
  |
```

Można to obejść używając `r#`:

```rust
let r#fn = 0;
```

## Mutowanie zmiennych

Domyślnie wartości zmiennych nie mogą ulec zmianie. Mówimy, że zmienna nie jest **mutowalna**.
Próba przypisania nowej wartości kończy się błędem kompilacji.

```rust,compile_fail
let thx = 1138;
println!("THX {thx}");
thx = 666;
```

Aby móc zmieniać wartość zmiennej, należy dodać słowo `mut` przed jej nazwą.

```rust
let mut thx = 1138;
println!("THX {thx}");
thx = 666;
```

## Stałe

Stałe to wartości, które nigdy nie ulegają zmianie.

```rust
const LIGHTSPEED: u32 = 299_792_458;
const DAY_IN_SEC: u32 = 24 * 60 * 60;
const PORT_HTTP: u16 = 80;
```

## Przysłanianie

Ponowna definicja zmiennej o tej samej nazwie powoduje, że poprzednia wartość przestaje istnieć.
Mówimy, że zmienna została przysłonięta.

```rust
let x = 10;
let x = x * 2 + 1;

// Tu, w nawiasach klamrowych, zaczyna się nowy zakres.
{
    let x = 100;
    println!("{x}"); // 100
    // Tutaj kończy się zakres.
}
// Zmienna `x` została odsłonięta do poprzedniej wartości.
println!("{x}"); // 21
```

Przysłanienie pozwala na utworzenie nowej zmiennej o tej samej nazwie ale innym typie.

```rust
let size = 69;
let size = 13u32;
let size = size * 2;
let size = "duży";
```

> Zmienne są częścią ramki stosu.
