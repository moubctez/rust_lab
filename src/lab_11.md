# Funkcje, wskaźniki do funkcji i domknięcia

### 1. Funkcja – przypomnienie składni

Klasyczna funkcja przyjmująca argument i zwracająca wynik:

```rust
fn square(x: i32) -> i32 {
    x * x
}

fn main() {
    let v = square(4);
    println!("{v}");
}
```

### 2. Funkcja – bez wartości zwracanej

Funkcja wykonująca efekt uboczny:

```rust
fn greet(name: &str) {
    println!("Witaj, {name}");
}

fn main() {
    greet("Jan B.");
}
```

---

### 3. Wskaźnik do funkcji – przekazywanie funkcji

Funkcja jako argument innej funkcji:

```rust
fn apply(f: fn(i32) -> i32, x: i32) -> i32 {
    f(x)
}

fn inc(x: i32) -> i32 {
    x + 1
}

fn main() {
    println!("{}", apply(inc, 5));
}
```

### 4. Wskaźnik do funkcji – wybór implementacji

Dynamiczny wybór funkcji:

```rust
fn add(x: i32) -> i32 { x + 1 }
fn mul(x: i32) -> i32 { x * 2 }

fn main() {
    let f: fn(i32) -> i32 = if true { add } else { mul };
    println!("{}", f(10));
}
```

### 5. Wskaźnik do funkcji – tablica funkcji

Przechowywanie funkcji w kolekcji:

```rust
fn inc(x: i32) -> i32 { x + 1 }
fn dec(x: i32) -> i32 { x - 1 }

fn main() {
    let ops: [fn(i32) -> i32; 2] = [inc, dec];
    println!("{}", ops[0](5));
    println!("{}", ops[1](5));
}
```

---

### 6. Domknięcie – proste użycie

Domknięcie bez przechwytywania kontekstu:

```rust
fn main() {
    let square = |x: i32| x * x;
    println!("{}", square(6));
}
```

### 7. Domknięcie – przechwytywanie zmiennej

Domknięcie korzystające z otaczającego kontekstu:

```rust
fn main() {
    let factor = 3;
    let mul = |x: i32| x * factor;
    println!("{}", mul(4));
}
```

### 8. Domknięcie – jako argument funkcji

Domknięcie przekazywane do funkcji wyższego rzędu:

```rust
fn apply<F: Fn(i32) -> i32>(f: F, x: i32) -> i32 {
    f(x)
}

fn main() {
    let result = apply(|x| x + 10, 5);
    println!("{result}");
}
```

### 9. Domknięcie – mutowalne

Domknięcie modyfikujące przechwycony stan:

```rust
fn main() {
    let mut counter = 0;
    let mut inc = || {
        counter += 1;
        counter
    };

    println!("{}", inc());
    println!("{}", inc());
}
```
