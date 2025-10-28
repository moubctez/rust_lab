# Dopasowywanie wzorców

Rust pozwala na przyrównanie wartości do zestawu możliwości i wykonanie kodu w zależności od
dopasowania. Do tego celu służy `match`.

```rust
enum Direction {
    North,
    South,
    East,
    West,
    Azimuth(f64),
    Point { longitude: f64, latitide: f64 },
}

fn azimuth(direction: Direction) -> f64 {
    match direction {
        Direction::North => 0.,
        Direction::South => 180.,
        Direction::East => 90.,
        Direction::West => 270.,
        Direction::Azimuth(azimuth) => azimuth,
        Direction::Point { .. } => f64::NAN, // Nie ma sensu, ale niech zostanie dla przykładu.
    }
}

fn print_direction(direction: Direction) {
    match direction {
        Direction::North => println!("North"),
        Direction::South => println!("South"),
        Direction::East => println!("East"),
        Direction::West => println!("West"),
        Direction::Azimuth(azimuth) => println!("Azimuth {azimuth}"),
        Direction::Point {
            longitude,
            latitide
        } => println!("Point {longitude} {latitide}"),
    }
}
```

`match` musi pokrywać wszystkie możliwości, jakie przyjmuje dana wartość. W przeciwnym razie
kompilator zwróci błąd.

```rust,compile_fail
fn fail(option: Option<u32>) {
    match option {
        Some(value) => println!("{value}"),
    }
}
```

```text
error[E0004]: non-exhaustive patterns: `None` not covered
 --> src/main.rs:2:11
  |
2 |     match option {
  |           ^^^^^^ pattern `None` not covered
```

Jak ogarnąć wszystkie przypadki w `match`? Przykłady:

```rust
let number = 5;
match number {
    13 => println!("pech"),
    21 => println!("oko"),
    60 => println!("kopa"),
    69 => println!(";)"),
    _ => println!("inna liczba")
}
```

```rust
let number = 13;
match number {
    13 => println!("pech"),
    21 => println!("oko"),
    60 => println!("kopa"),
    69 => println!(";)"),
    n @ 10..100 => println!("liczba dwucyfrowa {n}"),
    n if n > 1000 => println!("duża liczba {n}"),
    n => println!("liczba {n}")
}
```

## if-let

Zamiast

```rust
# enum Direction {
#     North,
#     South,
#     East,
#     West,
#     Azimuth(f64),
#     Point { longitude: f64, latitide: f64 },
# }
let dir = Direction::Azimuth(60.);
match dir {
    Direction::Azimuth(azimuth) => println!("Azymut {azimuth}"),
    _ => println!("Potrzebny azymut"),
}
```

lepiej jest użyć

```rust
# enum Direction {
#     North,
#     South,
#     East,
#     West,
#     Azimuth(f64),
#     Point { longitude: f64, latitide: f64 },
# }
let dir = Direction::Azimuth(60.);
if let Direction::Azimuth(azimuth) = dir {
    println!("Azymut {azimuth}");
} else {
    println!("Potrzebny azymut");
}
```

## let-else

```rust
# enum Direction {
#     North,
#     South,
#     East,
#     West,
#     Azimuth(f64),
#     Point { longitude: f64, latitide: f64 },
# }
fn azimuth_only(dir: Direction) -> f64 {
    let Direction::Azimuth(azimuth) = dir else {
        return f64::NAN;
    };
    azimuth
}

let dir = Direction::Azimuth(60.);
let azimuth = azimuth_only(dir);
```
