# Paczki, skrzynki i moduły

## Skrzynka

Skrzynka (ang. _crate_) to zbiór plików źródłowych, który kompiluje się do jednej postaci wynikowej.
Są dwie postaci skrzynek:

- binarna: powoduje utworzenie pliku wykonywalnego; musi posiadać funkcję `main`
- biblioteczka: powoduje utworzenie biblioteki procedur (którą z kolei można złączyć z jakimś innym
  programem wykonywanym)

## Pakiet

Pakiet jest zbiorem jednej lub więcej skrzynek; zawiera plik `Cargo.toml`, który mówi jak budować
skrzynki.

## Moduł

Kod źródłowy dzieli się na moduły, żeby pogrupować związane ze sobą części, zwiększyć czytelność
kodu, ograniczyć zasięg nazw oraz widoczność typów danych, funkcji i metod.

Kompilator rozpoznaje moduły na trzy równoznaczne sposoby:

- jako część pliku źródłowego, używając słowa `mod` i nawiasów klamrowych, np _src/main.rs_:

```rust
mod demo {
    pub struct Demo;
}

fn main() {
    let d = demo::Demo;
}
```

- w pliku _nazwa.rs_ (np. _src/demo.rs_)
- w pliku _nazwa/mod.rs_ (np. _src/demo/mod.rs_)

```rust
pub struct Demo;
```

Dla ostatnich dwóch sposobów, _src/main.rs_ powinien wyglądać następująca (zwróć uwagę na
konieczność podania deklaracji modułu z użyciem słowa `mod`):

```rust,ignore
mod demo;

fn main() {
    let d = demo::Demo;
}
```

Moduły mogą być zagnieżdżone, czyli jedne zawierają inne, np.

```rust
mod house {
    mod bedroom {
        fn sleep() {}
    }
    mod toilet {
        fn flush() {}
    }
}
```

lub w postaci plików:

```text
src/
└── main.rs
└── house
    └── mod.rs
    └── bedroom.rs
    └── toilet.rs
```

### Ścieżki

Ścieżka mówi, gdzie w gąszczu modułów i plików źródłowych można znaleźć interesującą nas rzecz.
Ścieżki mogą być

- względne: gdzie znaleźć coś relatywnie do bieżącego modułu
- bezwzględne: zaczynające się od korzenia skrzynki

```rust
mod demo {
    pub struct Demo;
}

fn main() {
    let d1 = demo::Demo; // ścieżka względna
    let d2 = crate::demo::Demo; // ścieżka bezwzględna
}
```

Można użyć słowa kluczowego `use` do zaimportowania rzeczy do obecnej przestrzeni nazw (ścieżki),
tak aby za każdym razem nie było konieczne podawanie pełnej ścieżki.

```rust
mod demo {
    pub struct Demo;
}

use demo::Demo;

fn main() {
    let d = Demo;
}
```
