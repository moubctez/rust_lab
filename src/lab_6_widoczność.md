# Widoczność

Domyślnie, wszystkie rzeczy są prywatne dla nadrzędnych modułów, ale widoczne dla modułów
podrzędnych. Poniższy przykład spowoduje błąd podczas kompilowania.

```rust,compile_fail
mod house {
    mod bedroom {
        fn sleep() {}
    }
    fn go_to_sleep() {
        bedroom::sleep();
    }
}
```

```text
error[E0603]: function `sleep` is private
 --> src/main.rs:6:18
  |
6 |         bedroom::sleep();
  |                  ^^^^^ private function
  |
```

Słowo kluczowe `pub` definiuje zakres widoczności. Dodanie go przed funkcją `sleep` spowoduje, że
kod skompiluje się poprawnie.

```rust
mod house {
    mod bedroom {
        pub fn sleep() {}
    }
    fn go_to_sleep() {
        bedroom::sleep();
    }
}
```

Domyślnie publiczne są:

- rzeczy powiązane z `pub Trait`
- warianty `pub enum`

## Ograniczenia widoczności

- `pub(in ścieżka)` ogranicza widoczność do podanej ścieżki; ścieżka musi być prostą ścieżką
  prowadzącą do nadrzędnego modułu i zaczynać się od `crate`, `self` lub `super`.
- `pub(crate)` ogranicza widoczność do bieżącej skrzynki.
- `pub(super)` ogranicza widoczność do modułu o jeden krop wyżej w hierarchii. (Działa tak samo jak
  `pub(in super)`).
- `pub(self)` ogranicza widoczność do bieżącego modułu, co jest jednoznaczne z `pub(in self)` lub
  nie podawanie `pub` w ogóle.

Przykład przedstawiony wyżej można też naprawić w taki sposób:

```rust
mod house {
    mod bedroom {
        pub(super) fn sleep() {}
    }
    fn go_to_sleep() {
        bedroom::sleep();
    }
}
```

Przykładowe ograniczenie widoczności struktury:

```rust,compile_fail
mod shape {
    pub struct Rectangle {
        width: f64,
        height: f64,
    }
}

use shape::Rectangle;

fn main() {
    let r = Rectangle {
        width: 5.0,
        height: 4.0,
    };
    println!("{}", r.width * r.height);
}
```

Powyższy kod nie skompiluje się ponieważ pola struktury są prywatne dla modułu `shape`.

```text
error[E0451]: fields `width` and `height` of struct `Rectangle` are private
  --> src/main.rs:23:9
   |
22 |     let r = Rectangle {
   |             --------- in this type
23 |         width: 5.0,
   |         ^^^^^ private field
24 |         height: 4.0,
   |         ^^^^^^ private field

error[E0616]: field `width` of struct `Rectangle` is private
  --> src/main.rs:24:22
   |
27 |     println!("{}", r.width * r.height);
   |                      ^^^^^ private field

error[E0616]: field `height` of struct `Rectangle` is private
  --> src/main.rs:24:32
   |
27 |     println!("{}", r.width * r.height);
   |                                ^^^^^^ private field
```

Poprawiony kod:

```rust
mod shape {
    pub struct Rectangle {
        width: f64,
        height: f64,
    }

    impl Rectangle {
        pub fn new(width: f64, height: f64) -> Self {
            Self { width, height }
        }

        pub fn area(&self) -> f64 {
            self.width * self.height
        }
    }
}

use shape::Rectangle;

fn main() {
    let r = Rectangle::new(5.0, 4.0);
    println!("{}", r.area());
}
```
