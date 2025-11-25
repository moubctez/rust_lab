# Kromki

Kromka (ang. _slice_) jest referencją do części elementów kolecji. Przykładem takiego zbioru
elementów może być ciąg znaków. Kromka nie posiada własności do danych, do których się odnosi.

## Zakresy

Na początek kilka sposobów na wydobycie zakresu ze zbioru elementów. Przykładowy kod:

```rust
let fib = [0, 1, 2, 3, 5, 8, 13, 21, 34, 55];
let slice = &fib[2..5];
for element in slice {
    println!("{element}");
}
```

> Podanie zakresu przekraczającego wielkość zbioru zakończy się paniką w trakcie wykonywania
> programu.

Przykład użycia różnego rodzaju zakresów (`assert_eq!` panikuje kiedy wartości są różne; tutaj
został użyty jedynie ilustracyjnie).

```rust
let arr = [0, 1, 2, 3, 4];
assert_eq!(arr[ ..  ], [0, 1, 2, 3, 4]); // std::ops::RangeFull
assert_eq!(arr[ .. 3], [0, 1, 2      ]); // std::ops::RangeTo
assert_eq!(arr[ ..=3], [0, 1, 2, 3   ]); // std::ops::RangeToInclusive
assert_eq!(arr[1..  ], [   1, 2, 3, 4]); // std::ops::RangeFrom
assert_eq!(arr[1.. 3], [   1, 2      ]); // std::ops::Range
assert_eq!(arr[1..=3], [   1, 2, 3   ]); // std::ops::RangeInclusive
```

Można używać zakresów do generowania liczb w pęli `for`:

```rust
let fib = [0, 1, 2, 3, 5, 8, 13, 21, 34, 55];
for index in 2..8 {
    println!("{}", fib[index]);
}
```

## &str

Typ `&str` jest kromką ciągu znaków. Ciąg znaków definiowany w kodzie ma właśnie typ `&str`.

```rust
let star_wars = "A long time ago in a galaxy, far, far away...";
```

Kromka ukrojona ze `String` jest również typu `&str`.

```rust
let hello_world = String::from("Hello, World!");
let hello = &hello_world[..5]; // &str
println!("{hello}");
```

> Dobrze jest się przyzwyczić, że `&str` występuje prawie zawsze w formie pożyczonej, czyli ubrany w
> ampersand `&`, a nie na golasa jako `str`.

Więc jaka jest różnica między `&String` a `&str`? Rozważmy poniższy przykład, którego kompilacja
kończy się błędem:

```rust,compile_fail
fn len(s: &String) -> usize {
    s.len()
}

fn main() {
    let hello_world = String::from("Hello, World!");
    println!("{}", len(&hello_world));
    println!("{}", len("hello"));
}
```

```text
error[E0308]: mismatched types
  --> src/main.rs:12:24
   |
12 |     println!("{}", len("hello"));
   |                    --- ^^^^^^^ expected `&String`, found `&str`
   |                    |
   |                    arguments to this function are incorrect
   |
   = note: expected reference `&String`
              found reference `&'static str`
```

Kiedy zmienimy typ parametru funkcji `len` na `&str` wszystko zadziała prawidłowo.

```rust
fn len(s: &str) -> usize {
    s.len()
}

fn main() {
    let hello_world = String::from("Hello, World!");
    println!("{}", len(&hello_world));
    println!("{}", len("hello"));
}
```

`&String` jest referencją do typu `String`. Nie jest ona jednoznaczna z referencją do kromki ciągu
znaków `&str`. Jednak można używać `&String` jako argumentu, którego typ jest `&str` ponieważ
`&String` potrafi automatycznie zmienić się w `&str` dzięki implementacji cechy `Deref`. Ale jest to
temat na przyszłe laboratorium.
