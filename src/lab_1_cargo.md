# Cargo

> Podręcznik [cargo](https://doc.rust-lang.org/cargo/index.html)

`cargo` jest narzędziem do zadządzania pakietami Rust.

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

Projekt jest gotowy do skompilowania:

```shell
% cd fajny_projekt
% cargo build
   Compiling fajny_projekt v0.1.0 (/Users/adam/Projects/rust/fajny_projekt)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.14s
```

Oraz do uruchomienia:

```shell
% cargo run
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.05s
     Running `target/debug/fajny_projekt`
Hello, world!
```

Sprawdzenie poprawności kodu:

```shell
% cargo check
    Checking fajny_projekt v0.1.0 (/Users/adam/Projects/rust/fajny_projekt)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.07s
```

Wyczyszczenie plików wynikowych (z katalogu _target_):

```shell
% cargo clean
     Removed 37 files, 1.0MiB total
```
