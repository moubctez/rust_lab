# Własne błędy

Cecha `std::error::Error` opisuje podstawowe oczekiwania co do błędów zwracanych jako wariant `Err`
z `Result`. Takie błędy muszą dodatkowo implementować cechy `Display` oraz `Debug`.

```rust
use std::{error::Error, fmt};

#[derive(Debug)]
struct MeaCulpa;

impl fmt::Display for MeaCulpa {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "mea maxima culpa")
    }
}

impl Error for MeaCulpa {}

fn demo() -> Result<i32, MeaCulpa> {
    "fee1".parse().map_err(|_| MeaCulpa)
}

fn main() {
    println!("{:?}", demo());
}
```

Zaimplementowanie cechy `Error` dla własnego typu błędu pozwala, na przykład, na zwrócenie
dynamicznego typu, czyli takiego, który będzie znany podczas uruchomienia programu, a nie w czasie
kompilacji. Dynamiczny typ musi zostać zapakowany w pudełko `Box`.

Następujący przykład różni się od poprzedniego tylko implementacją funkcji `demo`.

```rust
use std::{error::Error, fmt};

#[derive(Debug)]
struct MeaCulpa;

impl fmt::Display for MeaCulpa {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "mea maxima culpa")
    }
}

impl Error for MeaCulpa {}

fn demo() -> Result<i32, Box<dyn Error>> {
    "fee1".parse().map_err(|_| MeaCulpa.into())
}

fn main() {
    println!("{:?}", demo());
}
```
