# Cargo

> Podręcznik [cargo](https://doc.rust-lang.org/cargo/index.html)

`cargo` jest narzędziem do zadządzania pakietami Rust. Wszystkie czynności związane z projektem są
dostępne z `cargo`, w szczególności:

* dodawanie zależności
* budowanie (kompilowanie)
* testowanie
* sprawdzanie wydajności (ang. _benchmarking_)
* publikowanie

## Nowy projekt

Utworzenie nowego projektu

```shell
% cargo new fajny_projekt
    Creating binary (application) `fajny_projekt` package
note: see more `Cargo.toml` keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html
```

Nowy projekt będzie zawierał:

* Plik konfiguracyjny projektu _Cargo.toml_.
* Przykładowy kod programu _src/main.rs_.
* Dla [git](https://git-scm.com): katalog _.git_ oraz plik _.gitignore_.

Zawartość _Cargo.toml_:

```toml
[package]
name = "fajny_projekt"
version = "0.1.0"
edition = "2024"

[dependencies]
```

Zawartość _src/main.rs_:

```rust
fn main() {
    println!("Hello, world!");
}
```

## Budowanie

Projekt jest gotowy do skompilowania:

```shell
% cd fajny_projekt
% cargo build
   Compiling fajny_projekt v0.1.0 (/Users/adam/Projects/rust/fajny_projekt)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.14s
```

## Uruchamianie

Oraz do uruchomienia:

```shell
% cargo run
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.05s
     Running `target/debug/fajny_projekt`
Hello, world!
```

## Sprawdzanie

Sprawdzenie poprawności kodu (powinno być szybsze niż budowanie):

```shell
% cargo check
    Checking fajny_projekt v0.1.0 (/Users/adam/Projects/rust/fajny_projekt)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.07s
```

## Czyszczenie

Wyczyszczenie plików wynikowych (zostanie usunięta cała zawartość katalogu _target_):

```shell
% cargo clean
     Removed 37 files, 1.0MiB total
```

## Pomoc

Wszystkie komendy polecenia `cargo` uruchomione z opcją `--help` wyświetlają informację na temat
składni, na przykład:

```shell
% cargo new --help
Create a new cargo package at <path>

Usage: cargo new [OPTIONS] <PATH>

Arguments:
  <PATH>

Options:
      --vcs <VCS>                Initialize a new repository for the given version control system, overriding a global configuration. [possible values: git, hg,
                                 pijul, fossil, none]
      --bin                      Use a binary (application) template [default]
      --lib                      Use a library template
      --edition <YEAR>           Edition to set for the crate generated [possible values: 2015, 2018, 2021, 2024]
      --name <NAME>              Set the resulting package name, defaults to the directory name
      --registry <REGISTRY>      Registry to use
  -v, --verbose...               Use verbose output (-vv very verbose/build.rs output)
  -q, --quiet                    Do not print cargo log messages
      --color <WHEN>             Coloring [possible values: auto, always, never]
      --config <KEY=VALUE|PATH>  Override a configuration value
  -Z <FLAG>                      Unstable (nightly-only) flags to Cargo, see 'cargo -Z help' for details
  -h, --help                     Print help

Manifest Options:
      --locked   Assert that `Cargo.lock` will remain unchanged
      --offline  Run without accessing the network
      --frozen   Equivalent to specifying both --locked and --offline

Run `cargo help new` for more detailed information.
```

Uruchomienia `cargo help <nazwa polecenia>` wyświetla podręcznik dla danego polecenia.

## Zależności

Projekt może korzystać z innych pakietów. Pakiety domyślnie umieszczane są na witrynie
[crates.io](https://crates.io). Do projektu można dodać takie zależności poprzez:

* uruchomienie polecenia `cargo add`; na przykład:

```shell
% cargo add base64
    Updating crates.io index
      Adding base64 v0.22.1 to dependencies
             Features:
             + alloc
             + std
    Updating crates.io index
     Locking 1 package to latest Rust 1.90.0 compatible version
      Adding base64 v0.22.1
```

* ręczne dopisanie do pliku `Cargo.toml`

```toml
[dependencies]
base64 = "0.22.1"
```

Analogicznie, usunięcie zależności można dokonać poprzez:

* uruchomienie polecenia `cargo remove`, na przykład:

```shell
% cargo remove base64
    Removing base64 from dependencies
```

* usunięcie wpisu w pliku `Cargo.toml`.

### Uaktualnianie zależności

Z biegiem czasu nasza zależność może zostać uaktualniona do nowej wersji. Uruchamianie polecenia
`cargo update` uaktualni zależności, co zostanie zapisane w pliku `Cargo.lock`. Podanie wesji w
formacie `x.y.z` (np. `0.22.1`) uaktualni tylko wersje w zakresie `x.y` (np. z `0.22.1` do `0.22.2`,
ale nie do `0.23`).

> Można szczegółowo kontrolować zakres wersji. Informacje na ten temat można znaleźć w podręczniku
> do Cargo.

### Cechy

Pakiety mogą mieć pewne opcjonalne cechy (ang. _feature_), które zmieniają ich funkcjonalność. Na
przykład dla pakietu `uuid` można włączyć obsługę różnych standardów
[UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier). Dodanie zależności
odpowiednią cechą obsługuje opcja `-F --features`, na przykład:

```shell
% cargo add uuid --features v4
```

Spowoduje to dodanie wpisu do pliku `Cargo.toml`:

```toml
[dependencies]
uuid = { version = "1.18.1", features = ["v4"] }
```

Niektóre pakiety mają zestaw domyślnych cech. Jeżeli nie chcemy dodawać zależności z domyślnymi
cechami, należy podać opcję `--no-default-features`, na przykład:

```shell
% cargo add uuid --features v4 --no-default-features
```

W pliku `Cargo.toml` pojawi się:

```toml
[dependencies]
uuid = { version = "1.18.1", features = ["v4"], default-features = false }
```
