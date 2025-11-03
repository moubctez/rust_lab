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
