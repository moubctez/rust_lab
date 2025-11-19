# Zbiory i iteratory

Standardowa biblioteka Rust (std) implementuje użyteczne struktury zarządzające kolekcjami danych.

* Ciągi znaków: [String](https://doc.rust-lang.org/std/string/struct.String.html)
* Sekwencje: [Vec](https://doc.rust-lang.org/std/vec/struct.Vec.html),
  [VecDeque](https://doc.rust-lang.org/std/collections/struct.VecDeque.html),
  [LinkedList](https://doc.rust-lang.org/std/collections/struct.LinkedList.html)
* Mapy: [HashMap](https://doc.rust-lang.org/std/collections/struct.HashMap.html),
  [BTreeMap](https://doc.rust-lang.org/std/collections/struct.BTreeMap.html)
* Zbiory: [HashSet](https://doc.rust-lang.org/std/collections/struct.HashSet.html),
  [BTreeSet](https://doc.rust-lang.org/std/collections/struct.BTreeSet.html)
* Inne: [BinaryHeap](https://doc.rust-lang.org/std/collections/struct.BinaryHeap.html)

## Vec

Tworzenie wektora:

```rust
let mut primes = Vec::new();
primes.push(1);
primes.push(2);
primes.push(3);
primes.push(5);
primes.push(8);
```

lub za pomocą makro:

```rust
let primes = vec![1, 2, 3, 5, 8];
```

Pobieranie elementu za pomocą indeksu:

```rust
let primes = vec![1, 2, 3, 5, 8];
let second = primes[1];
```

Pobieranie elementu za pomocą metody `get`:

```rust
let primes = vec![1, 2, 3, 5, 8];
let index = 3;
match primes.get(index) {
    Some(value) => println!("{value}"),
    None => println!("Brak wartości pod indeksem {index}"),
}
```

Wielkość i pojemność:

```rust
let mut primes = Vec::with_capacity(10);
primes.extend_from_slice(&[1, 2, 3, 5, 8]);
println!("Wielkość {}", primes.len());
println!("Pojemność {}", primes.capacity());
```

## HashMap

`HashMap` przechowuje wartości pod kluczami. Klucze są unikalne.

Tworzenie mapy, pobieranie wartości po kluczu, usuwanie wartności:

```rust
use std::collections::HashMap;

let mut status_codes = HashMap::new();
status_codes.insert(200, "OK");
status_codes.insert(201, "Created");
status_codes.insert(302, "Found");
status_codes.insert(403, "Forbidden");

if let Some(status) = status_codes.get(&201) {
    println!("Pobrany: {status}");
}

if let Some(status) = status_codes.remove(&403) {
    println!("Usunięty: {status}");
}
```
