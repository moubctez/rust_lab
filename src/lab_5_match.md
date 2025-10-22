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

`match` musi pokrywać wszystkie możliwości, jakie przyjmuje dana wartość. W przeciwnym razie
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
