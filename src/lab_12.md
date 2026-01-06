# Testowanie

Testy powinny być umieszczone w osobnym module. Moduł ten nie będzie kompilowany z głównym programem
lub biblioteką. Podobnie jak inne moduły, może być w tym samym pliku co testowany kod, lub w osobnym.

> [!NOTE]
> Nazwa modułu nie ma znaczenia.

```rust
fn rev(tab: &mut [u32]) {
    tab.reverse();
}

fn main() {}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_reverse() {
        let mut fib = [1, 2, 3, 5, 8, 11];
        rev(&mut fib);
        assert_eq!(fib, [11, 8, 5, 3, 2, 1]);
        assert_eq!(fib.len(), 6, "niepoprawna długość tablicy");
        assert_ne!(fib[0], 0);
        assert!(fib.contains(&1));
    }
}
```

Funkcje testujące są ozdabiane adnotacją `#[test]`.

Makro `assert_eq!` porównuje ze sobą dwa argumenty. Jeżeli są różne, spowoduje panikę (`panic!`).
Podobnie `assert_ne!` z tym, że oczekuje, że oba argumenty są różne. Porównywane elementy muszą
implementować `PartialEq`. Makro `assert!` oczekuje, że argument zwróci warość `true`.

Można podać dodatkowy argument, który będzie tekstem wyświetlonym w przypadku błędu.

Testowanie projektu:

```shell
cargo test
```

> Można użyć skrótu: `cargo t`

## Używanie `?`

Testy mogą zwracać `Result`, a w kodzie testu można użyć `?`. W przypadku wystąpienia błędu, test
nie będzie zaliczony.

```rust
#[test]
fn test_open_file() -> Result<(), std::io::Error> {
    std::fs::File::open("/not_existant")?;
    Ok(())
}
```

## Ignorowanie testów

Domyślnie pomijane są testy oznaczone do zignorowania.

```rust
#[test]
#[ignore]
fn test_ignored() {
    assert_eq!(0, 0);
}
```

Można jednak uruchomić takie zignorowane testy dodając odpowiednie opcje do polecenia `cargo test`.

Opcja `--ignored` spowoduje wykonanie **jedynie** testów ignorowanych.

```shell
cargo test -- --ignored
```
Opcja `--include-ignored` spowoduje wykonanie zarówno wszystkich testów, włącznie ze zignorowanymi.

```shell
cargo test -- --include-ignored
```

## Testowanie panik

Jeźeli spodziewamy się, że testowany kod spanikuje, należy test oznaczyć jako `should_panic`.

```rust
#[test]
#[should_panic]
fn test_panic() {
    std::fs::File::open("/not_existant").unwrap();
}
```

## Zawężanie testowania

Na początek warto sprawdzić, ile jest testów zdefiniowanych w kodzie.

```shell
cargo test -- --list
```

Jest wiele sposobów na wybranie, które testy mają zostać wykonane.

- Testy biblioteki:

  ```shell
  cargo test --lib
  ```

- Testy programów wykonywalnych:

  ```shell
  cargo test --bin
  ```

- Wykonanie tylko testów, które zawierają podany ciąg znaków (włączając w to ścieżkę):

  ```shell
  cargo test -- reverse
  cargo test -- tests::
  ```

- Wykonanie tylko wskazanego testu:

  ```shell
  cargo test -- --exact tests::test_reverse
  ```

# 02_integration_tests

# Testy integracyjne

Domyślnie, testy integracyjne znajdują się w katalogu _tests_. Testowanie kodu odbywa się z punktu
widzenia użytkownika biblioteki. Oznacza to, że nie brane są pod uwagę oznaczania typu
`#[cfg(test)]`, a testy nie mają dostępu do prywatnych części kodu. Zasady oznaczania testów są
takie same jak w przypadku testów jednostkowych, czyli `#[test]` i podobne.

Przykładowe drzewo projektu z testem integracyjnym.

```text
projekt
├── Cargo.lock
├── Cargo.toml
├── src
│   └── lib.rs
└── tests
    └── integration1.rs
    └── integration2
        └── main.rs
```

> Aby wyłączyć automatyczne znajdowanie testów, w `Cargo.toml` należy ustawić `autotests = false`
> i zdefiniować sekcje `[[test]]`. Na przykład:
>
> ```toml
> autotests = false
>
> [[test]]
> name = "test_one"
> path = "integration_tests/test1.rs"
> ```

Testy (jednostkwe i integracyjne) są dodatkowo linkowane z zależonościami zdefiniowanymi w sekcji
`[dev-dependencies]`.

# 03_doc_tests

# Testy dokumentacji

Mają na celu utrzymanie poprawnej dokumentacji.

```rust
/// Funkcja odwracająca elementy w tablicy.
///
/// ```rust
/// let mut fib = [1, 2, 3, 5, 8, 11];
/// projekt::rev(&mut fib);
/// assert_eq!(fib, [11, 8, 5, 3, 2, 1]);
/// ```
pub fn rev(tab: &mut [u32]) {
    tab.reverse();
}
```

Domyślnie, testy dokumentacji są uruchamianie razem z innymi testami. Można to wyłączyć dopisując
do `Cargo.toml`:

```toml
[lib]
doctest = false
```

i uruchamiać takie testy ręcznie:

```shell
cargo test --doc
```

# 04_benches

# Testy wydajności

Domyślnie znajdują się w katalogu `benches`.

Przykładowe drzewo projektu z testami wydajności.

```text
projekt
├── Cargo.lock
├── Cargo.toml
└── benches
    └── bench1.rs
    └── bench2
        └── main.rs
├── src
│   └── lib.rs
```

> Standardowa biblioteka obsługuje testy wydajności tylko w wydaniu nocnym (_nightly_), więc
> znacznik `#[bench]` nie jest dostępny w wersji stabilnej.

## Przykład

Testy wydajności oparte o skrzynkę [Criterion](https://criterion-rs.github.io/).

Plik `src/lib.rs`:

```rust
pub fn gcd(mut a: usize, mut b: usize) -> usize {
    while b != 0 {
        (b, a) = (a % b, b);
    }
    a
}
```

Plik `benches/bench_gcd.rs`:

```rust,ignore
use std::hint::black_box;

use criterion::{Criterion, criterion_group, criterion_main};

use projekt::{gcd, gcd_sub};

fn gcd_benchmark(crit: &mut Criterion) {
    crit.bench_function("gcd", |b| {
        b.iter(|| gcd(black_box(9348759387540), black_box(3674)))
    });
}

criterion_group!(benches, gcd_benchmark);
criterion_main!(benches);
```

W `Cargo.toml`:

```toml
[dev-dependencies]
criterion = "0.8"

[[bench]]
name = "bench_gcd"
harness = false
```

Uruchomienie:

```shell
cargo bench
```

# 05_box

# Box

Typ danych, który przechowuje zawartość na stercie (nie na stosie). Na stosie trzymany jest
wskaźnik do danych. Kiedy typ wychodzi poza zakres, pamięć zarezerwowana na stercie zostaje
zwolniona.

```rust
let thx = Box::new(1138);
println!("THX {thx}");
```

`Box` jest przydatny przy typach danych, których wielkość nie jest znana w czasie kompilacji, oraz
takich, które zawierają się w sobie (rekursywnie).

```rust,compile_fail
#[derive(Debug)]
enum List<T> {
    Cons(T, List<T>),
    Nil,
}
```

```text
error[E0072]: recursive type `List` has infinite size
 --> src/main.rs:2:1
  |
2 | enum List<T> {
  | ^^^^^^^^^^^^
3 |     Cons(T, List<T>),
  |             ------- recursive without indirection
```

Poprawnie:

```rust
#[derive(Debug)]
enum List<T> {
    Cons(T, Box<List<T>>),
    Nil,
}

fn main() {
    let list = List::Cons(1, Box::new(List::Cons(2, Box::new(List::Nil))));
    println!("{list:?}");
}
```

Przeniesienie wartości ze sterty na stos przez _dereferencję_:

```rust
let boxed: Box<u16> = Box::new(1138);
let thx: u16 = *boxed;
```

# 06_deref

# Deref/DerefMut

Operator **dereferencji** (`*`) podąża za referencją i wyłuskuje z niej wartość.

```rust
let a = 1138; // typ: i32
let b = &a;   // typ: &i32

assert_eq!(a, *b);
```

Podobnie w przypadku `Box`:

```rust
let a = 1138;
let b = Box::new(a);

assert_eq!(a, *b);
```

Dereferencję umożliwia cecha `Deref` lub `DerefMut`. Dereferencja do innego typu danych zachodzi
automatycznie podczas kompilacji.

```rust
use std::ops::{Deref, DerefMut};

struct MyBox<T>(T);

impl<T> Deref for MyBox<T> {
    type Target = T;

    fn deref(&self) -> &Self::Target {
        &self.0
    }
}

impl<T> DerefMut for MyBox<T> {
    fn deref_mut(&mut self) -> &mut Self::Target {
        &mut self.0
    }
}

fn main() {
    let a = 1138;
    let mut b = MyBox(0);

    *b = 1138;
    assert_eq!(a, *b);
}
```

### Kiedy implementować

Powinno się implementować `Deref`/`DerefMut` kiedy:

- typ danych działa w podobny sposób jak inny typ, który zawiera w sobie;
- implementacja jest prosta;
- użytkownicy typu danych nie będą zaskoczeni zachowaniem dereferencji.

Nie powinno się implementować `Deref` kiedy:

- implementacja może nieoczekiwanie spowodować błąd;
- typ danych posiada metody kolidujące z zawieranym typem;
- nie jest wskazana dereferencja w publicznym API.

# 07_drop

# Drop

Cecha `Drop` jest używana w chwili kiedy wartość kończy żywot (wychodzi poza zakres). Przeważnie
jest używana do _posprzątania_ (zwoleniana przydziału pamięci lub zamknięcia użytych zasobów).

```rust
struct DropDemo;

impl Drop for DropDemo {
    fn drop(&mut self) {
        println!("Bye-bye");
    }
}

fn main() {
    let _ = DropDemo;
    println!("Exit");
}
```

Rust automatycznie wywoła metodę `drop`.

Nie jest dozwolone ręczne wywoływanie metody `drop`. Zamiast tego należy używać funkcji
`std::mem::drop`.

```rust
struct DropDemo;

impl Drop for DropDemo {
    fn drop(&mut self) {
        println!("Bye-bye");
    }
}

fn main() {
    let demo = DropDemo;
    drop(demo);
    println!("Exit");
}
```

> Nie można jednocześnie implementować `Drop` i `Copy`.

# 08_rc

# Rc

`Rc` umożliwia współdzielone referencje do zawartego typu. Typ umieszczony zostaje na stercie.
Każde wywołnie `clone` powoduje utworzenie nowego wskaźnika do tych samych danych. Kiedy nie będzie
żadnych odwołań do danych (wszystkie wyjdą poza zakres), to przydzielona pamięć zostanie zwolniona.

> `Rc` jest przeznaczony dla jednowątkowych programów.

```rust
use std::rc::Rc;

struct Master {
    name: String,
}

struct Slave {
    id: u64,
    master: Rc<Master>,
}

fn main() {
    let master = Rc::new(Master {
        name: "Hulk".into(),
    });
    let slave1 = Slave {
        id: 1,
        master: Rc::clone(&master),
    };
    let slave2 = Slave {
        id: 1,
        master: Rc::clone(&master),
    };
    println!("{}", Rc::strong_count(&master));

    drop(master);
    println!("{}", slave1.master.name);

    println!("{}", Rc::strong_count(&slave2.master));
}
```

Sposoby klonowania.

```rust
use std::rc::Rc;

let rc = Rc::new(());
let rc2 = rc.clone();
let rc3 = Rc::clone(&rc);
```

# 09_cell

# Cell, RefCell, OnceCell

Zasady bezpieczeństwa pamięci języka Rust:

- może być wiele niemutowalnych odwołań do obiektu (`&T`)
- może być tylko jedno mutowalne odwołanie do obiektu (`&mut T`)

są wymuszane przez kompilator. Czasami zachodzi potrzeba przeniesienia tych zasad na czas
wykonywania programu, ponieważ powyższe reguły nie są zbyt elastyczne.

## Cell

`Cell` pozwala na zmianę zawartości poprzez przenoszenie z lub do komórki. Oznacza to, że nigdy nie
można mieć `&mut T` do wewnętrznej wartości, a sama wartość nie może być pozyskana bez podmiany na
inną. Jeżeli zawartość implementuje `Copy`, można pobrać kopię zawartości metodą `get`.

W poniższym przykładzie zmienna `demo` nie jest mutowalna, ale można zmienić wartość pola `Cell`.

```rust
use std::cell::Cell;

struct Demo {
    value: Cell<u32>,
}

fn main() {
    let demo = Demo {
        value: Cell::new(1138),
    };
    demo.value.set(2001);
    demo.value.update(|v| v + 9);
    println!("{}", demo.value.get());
}
```

## RefCell

`RefCell` implementuje _dymaniczne pożyczanie_, czyli stosuje reguły pożycznia w czasie wykonywania
programu. Pożyczenie niemutowalne odbywa się metodą `borrow` – możliwe jest wiele pożyczeń.
Pożyczenie mutowalne daje metoda `borrow_mut` – można mieć tylko jedno takie pożyczenie. Złamanie
zasad powoduje panikę. 

```rust
use std::cell::RefCell;

struct Demo {
    value: RefCell<u32>,
}

fn main() {
    let demo = Demo {
        value: RefCell::new(1138),
    };
    {
        let v1 = demo.value.borrow();
        let v2 = demo.value.borrow();
        println!("{v1} == {v2}");
    }
    {
        let mut v = demo.value.borrow_mut();
        *v = 2001;
    }
    println!("{}", demo.value.borrow());
}
```

## OnceCell

`OnceCell` to komórka, która teoratycznie może być zapisana tylko raz. Pozawala na współdzieloną
referencję do zawartej wartości (bez kopiowania, jak to robi `Cell`) i bez reguł pożyczania (jak to
robi `RefCell`).

```rust
use std::cell::OnceCell;

fn main() {
    let demo = OnceCell::new();
    assert!(demo.get().is_none());

    let value: &String = demo.get_or_init(|| "Hello, World!".into());
    println!("{value}");
    assert!(demo.get().is_some());
}
```

