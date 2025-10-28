# Enumeracje

Enumeracje definiują nowy typ danych i wskazują jakie wartości może on przyjmować. Enumeracje
deklaruje się za pomocą słowa kluczowego `enum`.

```rust
enum Direction {
    North,
    South,
    East,
    West,
}

let direction = Direction::West;

fn go(direction: Direction) {
    //
}
```

## Jak w C

Każdy z wariantów enumaracji posiada swój numer, który wskazuje jaki wariant enumeracji przechowuje
dana zmienna. Taki numer nazywamy **wyróżnikiem**. Domyślnie warianty są numerowane od zera. Można
im przypisać własne wartości. Błędem jest przypisane tej samej wartości do różnych wariantów.

Pominięcie numeru wyróżnika spowoduje, że kompilator przypisze kolejną liczbę.

```rust
enum Direction {
    North = 1,
    South, // wyróżnik 2
    East = 5,
    West, // wyróżnik 6
}

let direction = Direction::West;
println!("{}", direction as u32);
```

## Powiązane wartości

Enumeracje mogą zawierać powiązane wartości. Takie powiązane wartości definiujemy podobnie jak
struktury – mogą one zawierać pola bez nazw lub pola z nazwami.

```rust
enum Direction {
    North,
    South,
    East,
    West,
    Azimuth(f64),
    Point { longitude: f64, latitide: f64 },
}
```

> Enumeracje z powiązanymi wartościami również posiadają wyróżniki.

Podobnie jak struktury, enumaracje mogą mieć zdefiniowane metody.

```rust
# enum Direction {
#     North,
#     South,
#     East,
#     West,
#     Azimuth(f64),
#     Point { longitude: f64, latitide: f64 },
# }
#
impl Direction {
    fn azimuth(&self) -> f64 {
        match self {
            Self::North => 0.,
            Self::South => 180.,
            Self::East => 90.,
            Self::West => 270.,
            Self::Azimuth(azimuth) => *azimuth,
            Self::Point { .. } => f64::NAN, // Nie ma sensu, ale niech zostanie dla przykładu.
        }
    }
}
```

## Option

Standardowa biblioteka Rust definiuje `Option` (dokładniej `std::option::Option`). Jest to
enumeracja mogąca przyjmować dwie wartości: _coś_ albo _nic_. Coś (`Some`) to konktetna wartość, a
nic (`None`) to brak wartości. Takie podejście do modelowania danych jest często użyteczne w
językach programowania (włączając w to SQL, który używa `NULL` jako brak wartości).

```rust
enum Option<T> {
    None,
    Some(T),
}
```

> `T` jest typem generycznym, o którym będzie mowa później.

Przykładowe użycie:

```rust
let dozen = Some(12u32);
let no_value: Option<i8> = None;
```

Standardowa biblioteka definiuje też zestaw metod dla `Option`. Na przykład, `as_slice` zamienia
`Option` na kromkę.

```rust
let dozen = Some(12u32);
for value in dozen.as_slice() {
    println!("{value}");
}
```
