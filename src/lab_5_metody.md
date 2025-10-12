# Metody

Funkcje ściśle powiązane ze strukturami danych nazywamy **metodami**. Metody są podobne do funkcji:
definiujemy je za pomocą słowa kluczowego `fn`. Różnica jest taka, że pierwszym parametrem jest
`&self` albo `&mut self`, oraz to, źe znajdują się w bloku `impl`.

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}
```
