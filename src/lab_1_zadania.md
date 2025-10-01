# Zadania do samodzielnego wykonania

1. Utwórz nowy projekt za pomocą `cargo`.

2. Komenda `cargo init` tworzy nowy projekt w istniejącym katalogu. Sprawdź jaka jest różnica między
   `cargo init` a `cargo new`?

3. Cargo obsługuje systemy obsługi wersji. Domyślnie nowy projekt jest tworzony ze wsparciem dla
   [Git](https://git-scm.com). Jak wyłączyć obsługę wersjonowania przy tworzeniu nowego projektu?
   Jak utworzyć projekt ze wsparciem dla [Mercurial](https://www.mercurial-scm.org)?

4. Zbuduj projekt używając `cargo build`. Jaki nowy katalog pojawił się w katalogu projektu? Co
   zwiera? Gdzie znajduje się główny plik wykonywalny?

5. Uruchom projekt używając `cargo run`. Czy potrzeba używać `cargo build` przed `cargo run`? Można
   to sprawdzić czyszcząc projekt za pomocą `cargo clean` przed budowaniem i uruchamianiem.

6. Spróbuj zbudować projekt z opcją `--release`. Czy zmieniła się wielkość pliku wynikowego?

7. Poniższy kod oblicza największy wspólny dzielnik. Spróbuj skompilować do postaci assemblera
   używając `rust --emit --crate-type lib plik.rs`. O ile mniej instrukcji procesora zostanie
   użytych przy włączonej optymalizacji (opcja `-O` lub `-C opt-level`).

```rust
pub fn gcd(mut a: usize, mut b: usize) -> usize {
    while b != 0 {
        (b, a) = (a % b, b);
    }
    a
}
```

8. Poniższy kod obilicza największy wspólny dzielnik bez użycia mnożenia. Spróbuj skompilować go do
   postaci assemblera i porównać z kodem powyżej. Który zawiera mniej instukcji procesora?

```rust
pub fn gcd_sub(mut a: usize, mut b: usize) -> usize {
    while a != b {
        if a > b { a -= b } else { b -= a }
    }
    a
}
```

9. Dodaj pakiet `base64` do zależności. Spróbuj zbudować i uruchomić poniższy program.

```rust,ignore
use base64::prelude::*;

fn main() {
    let encoded = BASE64_STANDARD.encode(b"sekret");
    println!("{encoded}");
}
```

10. Dodaj pakiet `uuid` do zależności z cechą `v4`. Spróbuj zbudować i uruchomić poniższy program.
   Czy za każdym uruchamianiem programu dostajemy taki sam wynik?

```rust,ignore
use uuid::Uuid;

fn main() {
    let id = Uuid::new_v4();
    println!("ID: {id}");
}
```
