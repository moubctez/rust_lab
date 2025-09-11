# Kompilator

> Podręcznik [rustc](https://doc.rust-lang.org/rustc/index.html).

Kompilatorem języka Rust jest `rustc`.
Skompilowanie program do postaci wykonywanlnej (binarnej) za pomocą kompilatora `rustc`:

```shell
% rustc program.rs
```

Domyślnie kompilator utworzy plik o takiej samej nazwie jak plik źródłowy, ale z pominięciem
rozszerzenia _.rs_ – w tym przypadku plik będzie nazwyć się `program`. (Na Windows będzie to
`program.exe`.) Można go uruchomić:

```shell
% ./program
Witaj, świecie!
```

Można wskazać inną nazwę pliku wynikowego używając opcji `-o`:

```shell
% rustc -o powitanie program.rs
```

## Kompilowanie dla innych architektur

Lista architektur wspieranych przez kompilator.

```shell
% rustc --print target-list
aarch64-apple-darwin
aarch64-apple-ios
aarch64-apple-ios-macabi
aarch64-apple-ios-sim
...
```

Kompilacja dla wskazanej architektury:

```shell
% rustc --target x86_64-apple-darwin program.rs
error[E0463]: can't find crate for `std`
  |
  = note: the `x86_64-apple-darwin` target may not be installed
  = help: consider downloading the target with `rustup target add x86_64-apple-darwin`

error: aborting due to 1 previous error
```

Brakuje komponentów potrzebnych dla wskazanej architektury – w tym przypadku macOS z procesorem
Intel.

```shell
% rustup target add x86_64-apple-darwin
```

Jeszcze raz:

```shell
% rustc --target x86_64-apple-darwin program.rs
% file program
program: Mach-O 64-bit x86_64 executable, flags:<NOUNDEFS|DYLDLINK|TWOLEVEL|PIE|HAS_TLV_DESCRIPTORS>
```

## Opcje generowania kodu

```shell
% rustc -Chelp
```

Przykład: kompilacja z optymalizacją, bez symboli dla debuggera.

```shell
% rustc -Copt-level=2 -Cstrip=symbols program.rs
```

Wynikowy plik powinien być mniejszy niż ten skompilowany z domyślnymi opcjami.

## Zaawansowane opcje generowania kodu

Kompilacja do assemblera:

```shell
rustc --emit asm program.rs
```
